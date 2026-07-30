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
| `USE_SIMPLE_AUTH` | No | `""` | Set to `"true"` to enable API key auth |
| `USE_SOCIAL_LOGIN` | No | `""` | Set to `"true"` to enable social login (OIDC/GitHub) |
| `API_KEY` | No | — | Secret key for `X-API-Key` header auth |
| `OIDC_ISSUER_URL` | No | — | OIDC issuer URL (e.g. `https://accounts.google.com`) |
| `OIDC_CLIENT_ID` | No | — | OIDC client ID |
| `GITHUB_CLIENT_ID` | No | — | GitHub OAuth App client ID |
| `JOB_RETENTION_DAYS` | No | `0` | Number of days to keep job history (`0` = never purge) |
| `JOB_STALLED_TIMEOUT` | No | `5` | Minutes after which a stuck `pending`/`running` job is auto-failed (worker crash guard) |
| `API_PROXY_FETCH` | No | `""` | Set to `"true"` to proxy thumbnail fetches through the API; thumbnails are embedded as base64 data URIs in search results (required when the frontend has no internet access) |
| `CORS_ALLOWED_ORIGINS` | No | `*` | Comma-separated allowed CORS origins |
| `CA_CERT` | No | — | Path to custom CA certificate bundle |

## Authentication

Three optional auth methods, independently togglable:

- **API key** — Set `USE_SIMPLE_AUTH=true` and configure `API_KEY`. Clients must send an `X-API-Key` header.
- **OIDC Bearer token** — Set `USE_SOCIAL_LOGIN=true`, `OIDC_ISSUER_URL`, and `OIDC_CLIENT_ID`. Tokens validated against the issuer's JWKS endpoint with key caching.
- **GitHub Bearer token** — Set `USE_SOCIAL_LOGIN=true` and `GITHUB_CLIENT_ID`. Tokens validated via the GitHub API.

The `/health` endpoint is always exempt from auth. If multiple methods are enabled, any one suffices. Authenticated users via Bearer token are recorded in the `requested_by` field on jobs.

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
