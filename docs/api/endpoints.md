# API Endpoints

## Health

```http
GET /health
```

Returns per-component health (Valkey, Celery worker, MariaDB). Always returns HTTP 200; check the `status` field for `"ok"` or `"degraded"`. Automatically purges jobs older than 30 days on every healthy check.

## Search

### Search YouTube Music

```http
GET /search?q={query}&type={albums|playlists}&limit={n}
```

| Parameter | Type | Default | Constraints | Description |
|---|---|---|---|---|
| `q` | string | — | required | Search query |
| `type` | string | `albums` | `albums` or `playlists` | Result type filter |
| `limit` | int | `10` | 1–50 | Max results |

Results are cached in Valkey for 5 minutes.

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
