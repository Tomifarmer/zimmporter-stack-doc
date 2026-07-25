# Helm Configuration

## Key Values

| Parameter | Description | Default |
|---|---|---|
| `api.replicaCount` | API server replicas | `1` |
| `api.image.tag` | API image tag | `latest` |
| `frontend.replicaCount` | Frontend replicas | `1` |
| `frontend.image.tag` | Frontend image tag | `latest` |
| `worker.replicaCount` | Celery worker replicas | `1` |
| `worker.concurrency` | Parallel downloads per worker | `4` |
| `mariadb.enabled` | Deploy MariaDB in-cluster | `true` |
| `redis.enabled` | Deploy Valkey in-cluster | `true` |
| `ingress.enabled` | Enable Ingress | `false` |
| `ingress.host` | Ingress hostname | — |
| `s3.endpoint` | S3 endpoint URL | — |
| `s3.bucket` | S3 bucket name | — |

## Storage Class

For production, configure `mariadb.persistence.storageClass` and `redis.persistence.storageClass` to use your cluster's default or a specific storage class.

## External Dependencies

Set `mariadb.enabled: false` and provide `mariadb.externalUrl` to use an existing MariaDB instance. Same for Redis/Valkey.
