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
|---|---|---|---|
| `api.replicas` | `1` | API server replicas |
| `api.resources` | `{requests: {cpu: 100m, memory: 128Mi}, limits: {cpu: 500m, memory: 512Mi}}` | Container resource limits/requests |
| `api.env.USE_SIMPLE_AUTH` | `"false"` | Enable API key authentication |
| `api.env.USE_SOCIAL_LOGIN` | `"false"` | Enable social login (OIDC/GitHub) Bearer token authentication |
| `api.env.CORS_ALLOWED_ORIGINS` | `"*"` | CORS allowed origins |
| `api.env.OIDC_ISSUER_URL` | `""` | OIDC issuer URL for token validation |
| `api.env.GITHUB_CLIENT_ID` | `""` | GitHub OAuth App client ID for token validation |
| `api.env.API_PROXY_FETCH` | `"false"` | Proxy thumbnail fetches through the API; thumbnails embedded as base64 data URIs in search results |
| `api.indexSource` | `"s3"` | Which library sources feed the available-albums index (`INDEX_SOURCE`): `s3` (default), `navidrome`, or `both` |
| `api.indexIntervalMinutes` | `30` | How often (minutes) the API pod dispatches the periodic library index scan |
| `api.extraEnv` | `[]` | Additional env vars for the API pod (see [extraEnv](#extra-environment-variables)) |
| `api.extraVolumes` | `[]` | Additional pod-level volumes (see [extraVolumes](#extra-volumes-volume-mounts)) |
| `api.extraVolumeMounts` | `[]` | Additional container volume mounts (see [extraVolumes](#extra-volumes-volume-mounts)) |

## Worker

| Parameter | Default | Description |
|---|---|---|---|
| `worker.replicas` | `1` | Celery worker replicas |
| `worker.resources` | `{requests: {cpu: 200m, memory: 256Mi}, limits: {cpu: 1, memory: 1Gi}}` | Container resource limits/requests |
| `worker.concurrency` | `4` | Parallel downloads per worker |
| `worker.pool` | `prefork` | Celery pool type |
| `worker.extraEnv` | `[]` | Additional env vars for the worker pod (see [extraEnv](#extra-environment-variables)) |
| `worker.extraVolumes` | `[]` | Additional pod-level volumes (see [extraVolumes](#extra-volumes-volume-mounts)) |
| `worker.extraVolumeMounts` | `[]` | Additional container volume mounts (see [extraVolumes](#extra-volumes-volume-mounts)) |

## Frontend

| Parameter | Default | Description |
|---|---|---|---|
| `frontend.replicas` | `1` | Frontend replicas |
| `frontend.resources` | `{requests: {cpu: 100m, memory: 128Mi}, limits: {cpu: 500m, memory: 512Mi}}` | Container resource limits/requests |
| `frontend.env.API_URL` | `""` | Backend API URL (auto-derived from in-cluster service when empty; set to full `https://` URL when using TLS ingress) |
| `frontend.env.USE_SOCIAL_LOGIN` | `"false"` | Enable social login (OIDC/GitHub) authentication |
| `frontend.env.USE_SIMPLE_AUTH` | `"false"` | Enable API key authentication |
| `frontend.env.OIDC_NAME` | `"OIDC"` | OIDC provider display name |
| `frontend.env.OIDC_ISSUER_URL` | `""` | OIDC issuer URL |
| `frontend.env.GITHUB_CLIENT_ID` | `""` | GitHub OAuth App client ID |
| `frontend.extraEnv` | `[]` | Additional env vars for the frontend pod (see [extraEnv](#extra-environment-variables)) |
| `frontend.extraVolumes` | `[]` | Additional pod-level volumes (see [extraVolumes](#extra-volumes-volume-mounts)) |
| `frontend.extraVolumeMounts` | `[]` | Additional container volume mounts (see [extraVolumes](#extra-volumes-volume-mounts)) |

## Ingress

| Parameter | Default | Description |
|---|---|---|
| `ingress.api.enabled` | `false` | Enable ingress for API |
| `ingress.api.host` | `api.example.com` | API hostname |
| `ingress.frontend.enabled` | `false` | Enable ingress for frontend |
| `ingress.frontend.host` | `app.example.com` | Frontend hostname |

## S3

| Parameter | Default | Description |
|---|---|---|---|
| `s3.endpoint` | `""` | S3 endpoint host:port |
| `s3.accessKey` | `""` | S3 access key (ignored when `existingSecret` is set) |
| `s3.secretKey` | `""` | S3 secret key (ignored when `existingSecret` is set) |
| `s3.bucket` | `""` | S3 bucket name |
| `s3.useSSL` | `false` | Use HTTPS for S3 |
| `s3.existingSecret` | `""` | Use existing secret instead of chart-generated s3 secret |
| `s3.existingSecretKeyMapping.accessKey` | `"access-key"` | Key for S3 access key in existing secret |
| `s3.existingSecretKeyMapping.secretKey` | `"secret-key"` | Key for S3 secret key in existing secret |

## Navidrome (optional index source)

When `api.indexSource` is `navidrome` or `both`, the worker queries
Navidrome's Subsonic API (`getAlbumList2`) to populate the available-albums
index — a tag-accurate view of the library.

| Parameter | Default | Description |
|---|---|---|
| `navidrome.url` | `""` | Navidrome base URL (worker `NAVIDROME_URL`) |
| `navidrome.user` | `""` | Subsonic API username (stored in the generated secret; see note below) |
| `navidrome.password` | `""` | Subsonic API password (worker `NAVIDROME_PASS`; ignored when `existingSecret` is set) |
| `navidrome.existingSecret` | `""` | Use existing secret instead of chart-generated navidrome secret |
| `navidrome.existingSecretKeyMapping.user` | `"user"` | Key for the username in the existing secret |
| `navidrome.existingSecretKeyMapping.password` | `"password"` | Key for the password in the existing secret |

`NAVIDROME_USER` and `NAVIDROME_PASS` are both injected into the worker from the
navidrome secret (`navidrome.existingSecret`, or the chart-generated one when
`navidrome.password` is set). When using an existing secret, the username is read from
the secret's `user` key — `navidrome.user` is **not** used.

## MariaDB

| Parameter | Default | Description |
|---|---|---|---|
| `mariadb.external.enabled` | `false` | Use external MariaDB instead of in-cluster |
| `mariadb.image` | `mariadb:11` | MariaDB image |
| `mariadb.resources` | `{requests: {cpu: 200m, memory: 512Mi}, limits: {cpu: 1, memory: 1Gi}}` | Container resource limits/requests |
| `mariadb.podSecurityContext` | `{runAsNonRoot: true, fsGroup: 999}` | Pod-level security context |
| `mariadb.persistence.size` | `10Gi` | PVC size |

## Valkey

| Parameter | Default | Description |
|---|---|---|---|
| `valkey.podSecurityContext` | `{runAsNonRoot: true, fsGroup: 999}` | Pod-level security context |
| `valkey.external.enabled` | `false` | Use external Valkey/Redis instead of in-cluster |
| `valkey.image` | `valkey/valkey:latest` | Valkey image |
| `valkey.resources` | `{requests: {cpu: 100m, memory: 128Mi}, limits: {cpu: 500m, memory: 512Mi}}` | Container resource limits/requests |
| `valkey.persistence.size` | `1Gi` | PVC size |

## POT Provider (bgutil-provider)

The chart deploys the [BgUtils yt-dlp POT provider](https://github.com/Brainicism/bgutil-ytdlp-pot-provider) and injects its URL into the worker via `POT_PROVIDER_URL`, enabling PO-token extraction for age-restricted content. Disabled by setting `potProvider.enabled: false`.

| Parameter | Default | Description |
|---|---|---|
| `potProvider.enabled` | `true` | Deploy the POT provider deployment + service |
| `potProvider.replicas` | `1` | Number of provider pods |
| `potProvider.image.repository` | `brainicism/bgutil-ytdlp-pot-provider` | Provider image |
| `potProvider.image.tag` | `1.3.1` | Provider image tag |
| `potProvider.port` | `4416` | HTTP port (service + container) |
| `potProvider.terminationGracePeriodSeconds` | `5` | Pod termination grace period (the provider doesn't exit gracefully on SIGTERM, so a short value avoids the default 30s hang) |
| `potProvider.probes.enabled` | `true` | Enable HTTP probes on `/ping` |
| `potProvider.resources` | `{requests: {cpu: 50m, memory: 64Mi}, limits: {cpu: 200m, memory: 256Mi}}` | Container resource limits/requests |
| `potProvider.podSecurityContext` | `{runAsNonRoot: true}` | Pod-level security context |
| `potProvider.nodeSelector` / `tolerations` / `affinity` / `extraEnv` | `{}` / `[]` / `{}` / `[]` | Scheduling constraints + extra env |

## Cookies (YouTube auth)

Cookies uploaded through the UI (`POST /cookies` on the API) are stored in **Valkey** (database 3) — the API writes them on upload and the worker reads them on each download job, writing a local writable copy for yt-dlp. No configuration or shared volume is required; the API and worker pods reach the same Valkey instance.

| Parameter | Default | Description |
|---|---|---|
| *(none)* | — | No cookie-specific values; content lives in Valkey under `zimmporter:cookies:content` / `zimmporter:cookies:meta` |

## Extra Volumes / Volume Mounts

Each component (`api`, `worker`, `frontend`) accepts `extraVolumes` and
`extraVolumeMounts` lists to mount additional volumes into the pod.

| Parameter | Default | Description |
|---|---|---|
| `api.extraVolumes` | `[]` | Applied to the API pod |
| `api.extraVolumeMounts` | `[]` | Applied to the API container |
| `worker.extraVolumes` | `[]` | Applied to the worker pod (in addition to default `temp-data`/`tmp` volumes) |
| `worker.extraVolumeMounts` | `[]` | Applied to the worker container (in addition to default mounts) |
| `frontend.extraVolumes` | `[]` | Applied to the frontend pod |
| `frontend.extraVolumeMounts` | `[]` | Applied to the frontend container |

Each entry follows the standard Kubernetes `volume` / `volumeMount` schema:

```yaml
frontend:
  extraVolumes:
    - name: config
      configMap:
        name: my-config
  extraVolumeMounts:
    - name: config
      mountPath: /etc/config
      readOnly: true
```

## Extra Environment Variables

Each component (`api`, `worker`, `frontend`) accepts an `extraEnv` list of
Kubernetes env var entries. A `global.extraEnv` list is also available and
is merged into **all** pods before each component-specific list (so component
vars take precedence over global ones).

| Parameter | Default | Description |
|---|---|---|
| `global.extraEnv` | `[]` | Applied to every pod |
| `api.extraEnv` | `[]` | Applied to the API pod only |
| `worker.extraEnv` | `[]` | Applied to the worker pod only |
| `frontend.extraEnv` | `[]` | Applied to the frontend pod only |

Each entry follows the standard Kubernetes `env` schema:

```yaml
extraEnv:
  - name: MY_VAR
    value: "plain value"
  - name: SECRET_VAR
    valueFrom:
      secretKeyRef:
        name: my-secret
        key: my-key
  - name: POD_IP
    valueFrom:
      fieldRef:
        fieldPath: status.podIP
```

## Storage Class

For production, configure `mariadb.storageClass` and `valkey.storageClass` to use your cluster's default or a specific storage class.

## External Dependencies

Set `mariadb.external.enabled: true` and provide `mariadb.external.host` / `mariadb.external.port` to use an existing MariaDB instance. Same for Valkey via `valkey.external.enabled`, `valkey.external.address`, and `valkey.external.port`.

## Authentication

| Parameter | Default | Description |
|---|---|---|
| `auth.apiKey` | `""` | API key for `X-API-Key` header (chart-generated `*-api-and-front-auth` secret) |
| `auth.apiKeyExistingSecret` | `""` | Name of an existing Secret containing the API key (overrides chart-generated secret) |
| `auth.apiKeyExistingSecretKey` | `"api-key"` | Key for the API key within `apiKeyExistingSecret` |
| `auth.oidcClientId` | `""` | OIDC client ID (injected into API + frontend; when set, overrides the ConfigMap value via the `*-auth-oidc` secret) |
| `auth.oidcClientSecret` | `""` | OIDC provider client secret (injected only when `frontend.env.USE_SOCIAL_LOGIN=true`) |
| `auth.githubClientId` | `""` | GitHub OAuth App client ID (injected into API + frontend; when set, overrides the ConfigMap value via the `*-auth-github` secret) |
| `auth.githubClientSecret` | `""` | GitHub OAuth App client secret (injected only when `frontend.env.USE_SOCIAL_LOGIN=true`) |
| `auth.authSecret` | `"dev-secret-change-in-production"` | NextAuth encryption key — signs JWTs and encrypts session cookies. Generate one with `openssl rand -base64 32`. (Injected only when `USE_SOCIAL_LOGIN=true`) |
| `auth.oidc.existingSecret` | `""` | Use existing secret instead of chart-generated `*-auth-oidc` |
| `auth.oidc.existingSecretKeyMapping.clientId` | `"client-id"` | Key for OIDC client ID in the existing secret |
| `auth.oidc.existingSecretKeyMapping.clientSecret` | `"client-secret"` | Key for OIDC client secret in the existing secret |
| `auth.github.existingSecret` | `""` | Use existing secret instead of chart-generated `*-auth-github` |
| `auth.github.existingSecretKeyMapping.clientId` | `"github-client-id"` | Key for GitHub client ID in the existing secret |
| `auth.github.existingSecretKeyMapping.clientSecret` | `"github-client-secret"` | Key for GitHub client secret in the existing secret |

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
