# API Overview

The Zimmporter API is a Python backend built with **FastAPI** that orchestrates music importing via an async task queue.

## Stack

- **Framework:** FastAPI with Uvicorn
- **Task Queue:** Celery (Valkey/Redis broker and result backend)
- **Database:** MariaDB via SQLAlchemy + PyMySQL
- **Core Libraries:** ytmusicapi, yt-dlp, ffmpeg, boto3 (S3), mutagen (metadata)
- **Testing:** pytest + pytest-mock + httpx

## Key Features

- Search YouTube Music for albums and playlists
- Download tracks with yt-dlp
- Convert audio to AAC via ffmpeg
- Embed metadata (title, artist, album, cover art) with mutagen
- Upload finished files to S3-compatible storage
- Track job status via Celery result backend
- Auth middleware (API key, OIDC Bearer token, or GitHub Bearer token)

## Project Structure

```
zimmporter-api/
├── api/                 # FastAPI application
│   ├── app.py           # App factory, lifespan, /health, auth middleware, CORS
│   ├── models.py        # Pydantic request/response schemas
│   └── routes/          # Route handlers (search, download, jobs)
├── db/                  # Database layer
│   ├── engine.py        # SQLAlchemy engine and session management
│   └── models.py        # ORM models (Job with requested_by, Song)
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

## Authentication

Three optional auth methods, independently togglable via env vars:

- **API key** — `USE_SIMPLE_AUTH=true` + `API_KEY`. Clients send `X-API-Key` header.
- **OIDC Bearer token** — `USE_SOCIAL_LOGIN=true` + `OIDC_ISSUER_URL` + `OIDC_CLIENT_ID`. Tokens validated against the issuer's JWKS endpoint.
- **GitHub Bearer token** — `USE_SOCIAL_LOGIN=true` + `GITHUB_CLIENT_ID`. Tokens validated via GitHub API.

The `/health` endpoint is always open. If multiple methods are enabled, **any** suffices. When a user authenticates via Bearer token, their identity is recorded in the `requested_by` field on jobs.
