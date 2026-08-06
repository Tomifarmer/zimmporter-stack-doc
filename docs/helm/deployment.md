# Deployment

## Install with Custom Values

```bash
helm install zimmporter oci://ghcr.io/tomifarmer/zimmporter \
  --set images.api.tag=v1.2.3 \
  --set s3.endpoint=https://s3.example.com:9000 \
  --set s3.accessKey=myAccessKey \
  --set s3.secretKey=mySecretKey \
  --set s3.bucket=music \
  --set ingress.api.enabled=true \
  --set ingress.api.host=zimmporter-api.example.com \
  --set ingress.frontend.enabled=true \
  --set ingress.frontend.host=zimmporter.example.com
```

## Upgrade

```bash
helm upgrade zimmporter oci://ghcr.io/tomifarmer/zimmporter \
  --set images.api.tag=v1.2.3 \
  --set images.worker.tag=v1.2.3 \
  --set images.frontend.tag=v1.2.3
```

## Uninstall

```bash
helm uninstall zimmporter
```

Persistent volume claims for Valkey and MariaDB must be deleted manually:

```bash
kubectl delete pvc -l app.kubernetes.io/instance=zimmporter
```

## Configuration via Values File

Create a `values.yaml`:

```yaml
images:
  api:
    tag: v1.2.3
  worker:
    tag: v1.2.3
  frontend:
    tag: v1.2.3

api:
  replicas: 2

frontend:
  env:
    # Must include the scheme (https://) when using TLS ingress,
    # otherwise the frontend treats it as a relative path.
    API_URL: "https://api.zimmporter.example.com"

ingress:
  api:
    enabled: true
    host: api.zimmporter.example.com
  frontend:
    enabled: true
    host: app.zimmporter.example.com

s3:
  endpoint: https://minio.example.com:9000
  accessKey: myAccessKey
  secretKey: mySecretKey
  bucket: music
  useSSL: true

mariadb:
  external:
    enabled: true
    host: mariadb.example.com
    port: 3306

valkey:
  external:
    enabled: true
    address: valkey.example.com
    port: 6379

database:
  rootPassword: strongRootPw
  password: strongUserPw
```

```bash
helm install zimmporter oci://ghcr.io/tomifarmer/zimmporter -f values.yaml
```
