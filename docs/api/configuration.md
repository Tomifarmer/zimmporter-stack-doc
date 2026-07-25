# API Configuration

## Environment Variables

| Variable | Required | Default | Description |
|---|---|---|---|
| `DB_HOST` | No | `localhost` | MariaDB hostname |
| `DB_PORT` | No | `3306` | MariaDB port |
| `DB_USER` | No | `root` | MariaDB user |
| `DB_PASS` | No | `root` | MariaDB password |
| `DB_NAME` | No | `zimmporter` | MariaDB database name |
| `CELERY_BROKER` | No | `redis://localhost:6379/0` | Valkey/Redis broker URL |
| `CELERY_BACKEND` | No | `redis://localhost:6379/1` | Valkey/Redis result backend URL |
| `AWS_ENDPOINT_URL` | Yes | — | S3-compatible endpoint URL |
| `AWS_ACCESS_KEY_ID` | Yes | — | S3 access key ID |
| `AWS_SECRET_ACCESS_KEY` | Yes | — | S3 secret access key |
| `AWS_BUCKET` | Yes | — | Target bucket name |
| `AWS_DEFAULT_REGION` | No | `us-east-1` | S3 region |
| `AWS_USE_SSL` | No | `true` | Use HTTPS for S3 connections |
| `REQUIRE_AUTH` | No | `""` | Set to `"true"` to enable API key auth |
| `API_KEY` | No | — | Secret key for `X-API-Key` header auth |
| `CORS_ALLOWED_ORIGINS` | No | `*` | Comma-separated allowed CORS origins |
| `CA_CERT` | No | — | Path to custom CA certificate bundle |

## Docker Compose

The repository includes a `docker-compose.yml` that starts:

- API server (port 8000)
- Celery worker
- MariaDB
- Valkey (Redis-compatible, used as Celery broker and result backend)

S3-compatible storage (e.g., MinIO) is expected to be available externally.

## Running Tests

```bash
uv sync --frozen
uv run python -m pytest tests/ -v
```
