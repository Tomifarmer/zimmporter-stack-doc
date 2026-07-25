# Deployment

## Install with Custom Values

```bash
helm install zimmporter ./zimmporter-helm \
  --set s3.endpoint=https://s3.example.com \
  --set s3.bucket=music \
  --set ingress.enabled=true \
  --set ingress.host=zimmporter.example.com
```

## Upgrade

```bash
helm upgrade zimmporter ./zimmporter-helm \
  --set api.image.tag=v1.2.3
```

## Uninstall

```bash
helm uninstall zimmporter
```

## Configuration via Values File

Create a `values.yaml`:

```yaml
api:
  image:
    tag: v1.2.3
  replicaCount: 2

ingress:
  enabled: true
  host: zimmporter.example.com

s3:
  endpoint: https://minio.example.com
  bucket: music
  region: us-east-1

mariadb:
  enabled: false
  externalUrl: mysql://user:password@mariadb.example.com:3306/zimmporter
```

```bash
helm install zimmporter ./zimmporter-helm -f values.yaml
```
