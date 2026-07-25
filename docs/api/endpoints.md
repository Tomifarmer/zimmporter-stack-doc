# API Endpoints

## Health

```http
GET /health
```

Returns per-component health (Valkey, Celery worker, MariaDB). Always returns HTTP 200; check the `status` field for `"ok"` or `"degraded"`.

## Search

### Search YouTube Music

```http
GET /search?q={query}&type={album|artist|playlist}&limit={n}
```

| Parameter | Type | Default | Description |
|---|---|---|---|
| `q` | string | — | Search query (required) |
| `type` | string | `album` | Result type filter |
| `limit` | int | `20` | Max results |

## Downloads

### Download Album

```http
POST /download/album
```

Request body:

```json
{
  "id": "browse_id_or_url",
  "concurrent": 4
}
```

### Download Playlist

```http
POST /download/playlist
```

Request body:

```json
{
  "id": "browse_id_or_url",
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

### Retry Failed Songs

```http
POST /jobs/{job_id}/retry
```

Retries all failed songs within a completed job.
