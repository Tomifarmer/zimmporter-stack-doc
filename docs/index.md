# Zimmporter Stack

A full-stack music import system. Search YouTube Music, download albums and playlists, convert audio to AAC, embed metadata and cover art, and upload the final files to an S3-compatible bucket.

## Components

| Component | Role | Stack |
|---|---|---|
| **API** | Backend service with async task queue | Python, FastAPI, Celery, MariaDB |
| **Frontend** | Web UI for searching and managing downloads | TypeScript, Next.js 16, React 19, PrimeReact |
| **Helm** | Kubernetes deployment chart | Helm 3 |

## Architecture Overview

```mermaid
graph LR
  U([User]) --> F[Frontend<br/>Next.js]
  F -->|REST| A[API<br/>FastAPI]
  A --> Q[(Redis)]
  Q --> W[Celery Workers]
  W --> YT[YouTube Music]
  W --> S3[(S3 Storage)]
  W --> DB[(MariaDB)]
```

The frontend communicates with the API via REST endpoints. The API dispatches long-running tasks (download, convert, upload) to Celery workers. Results and job status are stored in MariaDB.

## Repositories

- [zimmporter-api](https://github.com/Tomifarmer/zimmporter-api)
- [zimmporter-front](https://github.com/Tomifarmer/zimmporter-front)
- [zimmporter-helm](https://github.com/Tomifarmer/zimmporter-helm)
- [zimmporter-stack-doc](https://github.com/Tomifarmer/zimmporter-stack-doc)
