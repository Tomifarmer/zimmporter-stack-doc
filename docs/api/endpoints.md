# API Endpoints

## Health

```http
GET /health
```

Returns per-component health (Valkey, Celery worker, MariaDB). Always returns HTTP 200; check the `status` field for `"ok"` or `"degraded"`. Automatically purges jobs older than `JOB_RETENTION_DAYS` (default `0` — never purge) on every healthy check.

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

When `API_PROXY_FETCH=true` is set, the `thumbnail` field in each result is a **base64 data URI** (`data:image/jpeg;base64,...`) instead of a raw CDN URL. Thumbnails are fetched concurrently (up to 10 at a time) through the API's outbound proxy, cached in Valkey db 3 for 24 hours, and embedded directly into the response. This eliminates additional HTTP requests from the frontend for thumbnail images.

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
