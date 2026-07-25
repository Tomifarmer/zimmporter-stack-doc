# API Endpoints

## Search

### Search YouTube Music

```http
GET /api/search?query={query}&type={album|artist|playlist}
```

### Get Album Details

```http
GET /api/albums/{album_id}
```

## Import Jobs

### Create Import Job

```http
POST /api/jobs
```

Request body:

```json
{
  "source": "youtube_music",
  "url": "https://music.youtube.com/...",
  "format": "aac"
}
```

### Get Job Status

```http
GET /api/jobs/{job_id}
```

### List Jobs

```http
GET /api/jobs?status={status}&limit={n}
```

### Cancel Job

```http
POST /api/jobs/{job_id}/cancel
```

## Tracks

### List Tracks

```http
GET /api/tracks?album_id={album_id}
```

### Get Track Details

```http
GET /api/tracks/{track_id}
```

## Health

```http
GET /health
```

Returns service health status including database and broker connectivity.
