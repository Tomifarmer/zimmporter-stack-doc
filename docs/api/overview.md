# API Overview

The Zimmporter API is a Python backend built with **FastAPI** that orchestrates music importing via an async task queue.

## Stack

- **Framework:** FastAPI with Uvicorn
- **Task Queue:** Celery (Valkey/Redis broker and result backend)
- **Database:** MariaDB via SQLAlchemy + PyMySQL
- **Core Libraries:** ytmusicapi, yt-dlp, ffmpeg, boto3 (S3), mutagen (metadata)
- **Testing:** pytest + pytest-mock + httpx

## Key Features

- Search YouTube Music for albums, artists, and playlists
- Download tracks with yt-dlp
- Convert audio to AAC via ffmpeg
- Embed metadata (title, artist, album, cover art) with mutagen
- Upload finished files to S3-compatible storage
- Track job status via Celery result backend
- Auth middleware (optional API key via `X-API-Key` header)

## Project Structure

```
zimmporter-api/
├── api/                 # FastAPI application
│   ├── app.py           # App factory, lifespan, /health, auth, CORS
│   ├── models.py        # Pydantic request/response schemas
│   └── routes/          # Route handlers (search, download, jobs)
├── db/                  # Database layer
│   ├── engine.py        # SQLAlchemy engine and session management
│   └── models.py        # ORM models (Job, Song)
├── tasks/               # Celery configuration and task definitions
│   ├── celery_app.py    # Celery app configuration
│   └── download.py      # Album/playlist download tasks
├── zimmporter/          # Core library
│   ├── core.py          # YouTube search and download orchestration
│   ├── postprocessors.py# FFmpeg conversion, metadata embedding, S3 upload
│   ├── cert.py          # Custom CA certificate support
│   └── _version.py      # Version string
├── tests/               # pytest test suite
├── Dockerfile           # API server image (Debian)
├── Dockerfile.alpine    # API server image (Alpine, used in docker-compose)
├── Dockerfile.worker    # Celery worker image (Debian)
├── Dockerfile.worker.alpine # Celery worker image (Alpine)
└── pyproject.toml       # Project configuration and dependencies
```
