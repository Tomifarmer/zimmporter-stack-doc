# Helm Overview

The Zimmporter Helm chart deploys the full stack to Kubernetes.

## Resources Created

- **Deployments:** API server, Celery worker, Frontend
- **StatefulSets:** MariaDB, Redis/Valkey (optional, can use external)
- **Services:** Internal cluster networking
- **Ingress:** External access configuration
- **PersistentVolumeClaims:** Database and Redis storage
- **ConfigMaps/Secrets:** Application configuration

## Prerequisites

- Kubernetes 1.24+
- Helm 3
- S3-compatible storage accessible from the cluster

## Quick Install

```bash
helm install zimmporter oci://ghcr.io/tomifarmer/zimmporter
```
