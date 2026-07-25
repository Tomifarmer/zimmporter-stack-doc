# Architecture

## Data Flow

```mermaid
graph TD
  classDef primary fill:#3b82f6,stroke:#3b82f6,color:#fff
  classDef accent fill:#40e0d0,stroke:#40e0d0,color:#fff
  classDef storage fill:#1e293b,stroke:#3b82f6,color:#fff
  classDef external fill:#334155,stroke:#64748b,color:#fff

  CR[Client Request] --> R[FastAPI Router]
  R --> CT[Celery Task]
  CT --> WP[Worker Process]
  WP --> YT[YouTube Music<br/>search / download]
  YT --> FF[ffmpeg<br/>convert to AAC]
  FF --> MT[mutagen<br/>embed metadata]
  MT --> S3[boto3<br/>upload to S3]
  S3 --> DB[(MariaDB<br/>store result)]

  class CR,R,CT,WP primary
  class YT,FF,MT external
  class S3,DB storage
```

## Concurrency Model

The API uses a **concurrent worker pool** via the `billiard` library to manage multiple download/convert/upload operations in parallel within each Celery worker process. This allows efficient use of I/O-bound operations.

- **Celery workers** consume tasks from Redis/Valkey
- **Billiard pool** manages subprocesses for parallel downloads
- **FFmpeg** runs as a subprocess for audio conversion

## Database Schema

Key tables:

- **jobs** — tracks import job lifecycle (pending, running, completed, failed)
- **tracks** — stores individual track metadata and S3 paths
- **albums** — album-level metadata and cover art references

## S3 Path Convention

Uploaded files follow this path pattern:

```
{artist}/{album}/{track_number} - {track_title}.m4a
```
