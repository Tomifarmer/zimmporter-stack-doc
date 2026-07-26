# Helm Configuration

## Images

| Parameter | Default | Description |
|---|---|---|
| `images.api.repository` | `ghcr.io/tomifarmer/zimmporter-api` | API Docker image |
| `images.api.tag` | `latest` | API image tag |
| `images.worker.repository` | `ghcr.io/tomifarmer/zimmporter-worker` | Worker Docker image |
| `images.worker.tag` | `latest` | Worker image tag |
| `images.frontend.repository` | `ghcr.io/tomifarmer/zimmporter-front` | Frontend Docker image |
| `images.frontend.tag` | `latest` | Frontend image tag |

## API

| Parameter | Default | Description |
|---|---|---|
| `api.replicas` | `1` | API server replicas |
| `api.env.USE_SIMPLE_AUTH` | `"false"` | Enable API key authentication |
| `api.env.USE_SOCIAL_LOGIN` | `"false"` | Enable social login (OIDC/GitHub) Bearer token authentication |
| `api.env.CORS_ALLOWED_ORIGINS` | `"*"` | CORS allowed origins |
| `api.env.OIDC_ISSUER_URL` | `""` | OIDC issuer URL for token validation |
| `api.env.OIDC_CLIENT_ID` | `""` | OIDC client ID (audience) |
| `api.env.GITHUB_CLIENT_ID` | `""` | GitHub OAuth App client ID for token validation |

## Worker

| Parameter | Default | Description |
|---|---|---|
| `worker.replicas` | `1` | Celery worker replicas |
| `worker.concurrency` | `4` | Parallel downloads per worker |
| `worker.pool` | `prefork` | Celery pool type |

## Frontend

| Parameter | Default | Description |
|---|---|---|
| `frontend.replicas` | `1` | Frontend replicas |
| `frontend.env.API_URL` | `""` | Backend API URL (auto-derived when empty) |
| `frontend.env.USE_SOCIAL_LOGIN` | `"false"` | Enable social login (OIDC/GitHub) authentication |
| `frontend.env.USE_SIMPLE_AUTH` | `"false"` | Enable API key authentication |
| `frontend.env.OIDC_NAME` | `"OIDC"` | OIDC provider display name |
| `frontend.env.OIDC_ISSUER_URL` | `""` | OIDC issuer URL |
| `frontend.env.OIDC_CLIENT_ID` | `""` | OIDC client ID |
| `frontend.env.GITHUB_CLIENT_ID` | `""` | GitHub OAuth App client ID |

## Ingress

| Parameter | Default | Description |
|---|---|---|
| `ingress.api.enabled` | `false` | Enable ingress for API |
| `ingress.api.host` | `api.example.com` | API hostname |
| `ingress.frontend.enabled` | `false` | Enable ingress for frontend |
| `ingress.frontend.host` | `app.example.com` | Frontend hostname |

## S3

| Parameter | Default | Description |
|---|---|---|
| `s3.endpoint` | `""` | S3 endpoint host:port |
| `s3.accessKey` | `""` | S3 access key |
| `s3.secretKey` | `""` | S3 secret key |
| `s3.bucket` | `""` | S3 bucket name |
| `s3.useSSL` | `false` | Use HTTPS for S3 |

## MariaDB

| Parameter | Default | Description |
|---|---|---|
| `mariadb.external.enabled` | `false` | Use external MariaDB instead of in-cluster |
| `mariadb.image` | `mariadb:11` | MariaDB image |
| `mariadb.persistence.size` | `10Gi` | PVC size |

## Valkey

| Parameter | Default | Description |
|---|---|---|
| `valkey.external.enabled` | `false` | Use external Valkey/Redis instead of in-cluster |
| `valkey.image` | `valkey/valkey:latest` | Valkey image |
| `valkey.persistence.size` | `1Gi` | PVC size |

## Storage Class

For production, configure `mariadb.storageClass` and `valkey.storageClass` to use your cluster's default or a specific storage class.

## External Dependencies

Set `mariadb.external.enabled: true` and provide `mariadb.external.host` / `mariadb.external.port` to use an existing MariaDB instance. Same for Valkey via `valkey.external.enabled`, `valkey.external.address`, and `valkey.external.port`.

## Authentication

| Parameter | Default | Description |
|---|---|---|
| `auth.apiKey` | `""` | API key for `X-API-Key` header |
| `auth.oidcClientSecret` | `""` | OIDC provider client secret |
| `auth.githubClientSecret` | `""` | GitHub OAuth App client secret |
| `auth.authSecret` | `"dev-secret-change-in-production"` | NextAuth encryption secret |
| `auth.existingSecret` | `""` | Use existing secret instead of chart-generated one |

## Celery

| Parameter | Default | Description |
|---|---|---|
| `celery.broker` | `""` | Broker URL (auto-derived from Valkey when empty) |
| `celery.backend` | `""` | Result backend URL (auto-derived from Valkey when empty) |

## Database

| Parameter | Default | Description |
|---|---|---|
| `database.host` / `database.port` | `""` | Override DB connection (auto-resolves from MariaDB settings) |
| `database.rootPassword` | `""` | MariaDB root password |
| `database.user` | `zimmporter` | Application DB user |
| `database.password` | `""` | Application DB password |
| `database.name` | `zimmporter` | Database name |
| `database.existingSecret` | `""` | Use existing secret instead of chart-generated one |
