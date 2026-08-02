# API Endpoints

## Health

```http
GET /health
```

Returns per-component health (Valkey, Celery worker, MariaDB). Always returns HTTP 200; check the `status` field for `"ok"` or `"degraded"`. Automatically purges jobs older than `JOB_RETENTION_DAYS` (default `0` — never purge) on every healthy check. Also fails jobs stuck in `pending`/`running` longer than `JOB_STALLED_TIMEOUT` minutes (default `5`) — this recovers jobs orphaned when a worker crashed (e.g. OOM / SIGKILL), since their exception handler never ran.

## Search

### Search YouTube Music

```http
GET /search?q={query}&type={albums|featured_playlists|community_playlists}&limit={n}
```

| Parameter | Type | Default | Constraints | Description |
|---|---|---|---|---|
| `q` | string | — | required | Search query |
| `type` | string | `albums` | `albums`, `featured_playlists`, or `community_playlists` | Result type filter |
| `limit` | int | `10` | 1–50 | Max results |

Results are cached in Valkey for 5 minutes.

Each result includes an `available` boolean that flags albums/playlists already present in the library. Matching is by exact YT Music `browse_id` (recorded from successful download jobs) or normalized artist+title, sourced from the `available_albums` table maintained by the periodic library index scan (S3 and/or Navidrome, selected via `INDEX_SOURCE`).

When `API_PROXY_FETCH=true` is set, the `thumbnail` field in each result is a **base64 data URI** (`data:image/jpeg;base64,...`) instead of a raw CDN URL. Thumbnails are fetched concurrently (up to 10 at a time) through the API's outbound proxy, cached in Valkey db 3 for 24 hours, and embedded directly into the response. This eliminates additional HTTP requests from the frontend for thumbnail images.

## Cookies

### Get Cookie Status

```http
GET /cookies
```

Returns metadata about the configured yt-dlp cookies file — never its contents.

| Field | Type | Description |
|---|---|---|
| `exists` | bool | Whether a cookies file is present |
| `size` | int | File size in bytes |
| `cookie_count` | int | Number of parsed cookies |
| `domains` | array | Cookie domains (e.g. `.youtube.com`) |
| `modified_at` | string \| null | Last file modification timestamp |
| `is_stale` | bool | `true` when the backend detected the cookies are no longer valid |

### Upload Cookies

```http
POST /cookies
```

Multipart upload (field `file`) of a Netscape-format `cookies.txt`. Validates that the file parses and contains at least one `youtube.com` cookie, and is at most 2 MB. The file is written atomically into the shared cookies volume (`COOKIE_DIR`), so workers pick it up without restart.

While stale, downloads run anonymously (bad cookies are skipped) and `GET /cookies` reports `is_stale: true`. Uploading a fresh file clears the flag.

| Status | Condition |
|---|---|
| `200` | Cookies stored and applied |
| `400` | Missing/invalid file, no YouTube cookie, or larger than 2 MB |

## Thumbnail Proxy

```http
GET /thumbnail?url={encoded_cdn_url}
```

| Parameter | Type | Default | Constraints | Description |
|---|---|---|---|---|
| `url` | string | — | required | URL-encoded CDN thumbnail URL |
| `X-Cache` | (response header) | — | — | `HIT` or `MISS` — whether the response was served from Valkey cache |
| `Cache-Control` | (response header) | — | — | `public, max-age=86400` |

Fetches a thumbnail image from the upstream CDN, caches it in Valkey db 3 for 24 hours, and returns the raw image bytes with the original content type. Max image size: 10 MB. Returns 502 on upstream failure, 400 when `url` is missing.

This endpoint is **excluded from auth middleware** so `<img>` tags can load thumbnails without authentication headers. It is primarily used by external integrations; the search route embeds thumbnails as data URIs when `API_PROXY_FETCH=true`.

## Downloads

### Download Album

```http
POST /download/album
```

| Field | Type | Default | Constraints | Description |
|---|---|---|---|---|
| `id` | string | — | required | Comma-separated album browse IDs |
| `concurrent` | int | `4` | 1–32 | Parallel downloads per album |

Request body:

```json
{
  "id": "MPREb_xxx,MPREb_yyy",
  "concurrent": 4
}
```

### Download Playlist

```http
POST /download/playlist
```

| Field | Type | Default | Constraints | Description |
|---|---|---|---|---|
| `id` | string | — | required | Comma-separated playlist browse IDs |
| `concurrent` | int | `4` | 1–32 | Parallel downloads per playlist |

Request body:

```json
{
  "id": "VLx_xxxxx",
  "concurrent": 4
}
```

## Jobs

### Get Job Status

```http
GET /jobs/{job_id}
```

Returns job metadata, current progress (album, song counts), and per-song status.

### List Jobs

```http
GET /jobs?limit={n}&offset={n}
```

| Parameter | Type | Default | Description |
|---|---|---|---|
| `limit` | int | `50` | Maximum jobs to return |
| `offset` | int | `0` | Number of jobs to skip |

When authenticated via Bearer token (OIDC/GitHub), only the requesting user's jobs are returned. Unauthenticated or API-key requests see all jobs.

### Retry Failed Songs

```http
POST /jobs/{job_id}/retry
```

Resets all failed songs in a job to `pending` and re-dispatches the original Celery task.

| Status | Condition |
|---|---|
| `200` | Job retry dispatched |
| `400` | No failed songs to retry |
| `403` | Job belongs to a different OIDC user |
| `404` | Job not found |
