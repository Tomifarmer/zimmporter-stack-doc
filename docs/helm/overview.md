# Helm Overview

The Zimmporter Helm chart deploys the full stack to Kubernetes.

## Resources Created

- **Deployments:** API server, Celery worker, Frontend, BgUtils POT provider (optional)
- **StatefulSets:** MariaDB, Valkey (optional, can use external)
- **Services:** ClusterIP for each component (4–6 depending on external dependencies and the POT provider)
- **Ingress:** API and Frontend (optional, separately configurable)
- **ConfigMaps:** API config, Worker config, Frontend config (non-sensitive env vars)
- **PersistentVolumeClaims:** Cookies (shared RWX volume for yt-dlp cookies), plus MariaDB and Valkey
- **Secrets:** Database credentials (optional), S3 credentials, Navidrome credentials (optional), API and frontend auth, OIDC credentials (optional), GitHub OAuth credentials (optional)

The API pod also runs the periodic library index dispatcher (`INDEX_INTERVAL_MINUTES`, default 30; `INDEX_SOURCE` selects `s3`, `navidrome`, or `both`).

The API, worker, and frontend deployments carry a `checksum/config` annotation, so changing any ConfigMap value (e.g. `api.indexSource` or `navidrome.*`) automatically rolls the affected pods after `helm upgrade` — no manual restart needed.

## Prerequisites

- Kubernetes 1.25+
- Helm 3.8+
- Default StorageClass (or set one explicitly for Valkey / MariaDB / cookies)
- A StorageClass with `ReadWriteMany` access (or a provider that supports shared volumes) for the cookies volume — both the API and worker pods mount it
- S3-compatible storage accessible from the cluster

## Quick Install

```bash
helm install zimmporter oci://ghcr.io/tomifarmer/zimmporter \
  --set s3.endpoint=s3.example.com:9000 \
  --set s3.accessKey=myAccessKey \
  --set s3.secretKey=mySecretKey \
  --set s3.bucket=myBucket \
  --set database.rootPassword=strongRootPw \
  --set database.password=strongUserPw
```
