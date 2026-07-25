# API Configuration

## Environment Variables

| Variable | Required | Default | Description |
|---|---|---|---|
| `DATABASE_URL` | Yes | — | MariaDB connection string |
| `CELERY_BROKER_URL` | Yes | — | Redis/Valkey URL |
| `S3_ENDPOINT` | Yes | — | S3-compatible endpoint URL |
| `S3_ACCESS_KEY` | Yes | — | S3 access key ID |
| `S3_SECRET_KEY` | Yes | — | S3 secret access key |
| `S3_BUCKET` | Yes | — | Target bucket name |
| `S3_REGION` | No | `us-east-1` | S3 region |
| `LOG_LEVEL` | No | `INFO` | Logging level |
| `WORKER_CONCURRENCY` | No | `4` | Parallel downloads per worker |

## Docker Compose

The repository includes a `docker-compose.yml` that starts:

- API server (port 8000)
- Celery worker
- MariaDB
- Redis (Valkey)
- MinIO (S3-compatible local storage)

## Running Tests

```bash
uv sync --frozen
uv run python -m pytest tests/ -v
```
