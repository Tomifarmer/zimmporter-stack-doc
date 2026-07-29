# Helm Overview

The Zimmporter Helm chart deploys the full stack to Kubernetes.

## Resources Created

- **Deployments:** API server, Celery worker, Frontend
- **StatefulSets:** MariaDB, Valkey (optional, can use external)
- **Services:** ClusterIP for each component (4 when in-cluster, 2 when external)
- **Ingress:** API and Frontend (optional, separately configurable)
- **ConfigMaps:** API config, Worker config, Frontend config (non-sensitive env vars)
- **Secrets:** Database credentials (optional), S3 credentials, API and frontend auth, OIDC credentials (optional), GitHub OAuth credentials (optional)

## Prerequisites

- Kubernetes 1.25+
- Helm 3.8+
- Default StorageClass (or set one explicitly for Valkey / MariaDB)
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
