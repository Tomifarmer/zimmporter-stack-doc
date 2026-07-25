# API Overview

The Zimmporter API is a Python backend built with **FastAPI** that orchestrates music importing via an async task queue.

## Stack

- **Framework:** FastAPI with Uvicorn
- **Task Queue:** Celery (Redis/Valkey broker)
- **Database:** MariaDB via SQLAlchemy + PyMySQL
- **Core Libraries:** ytmusicapi, yt-dlp, ffmpeg, boto3 (S3), mutagen (metadata)
- **Testing:** pytest + pytest-mock + httpx (71+ tests)

## Key Features

- Search YouTube Music for albums, artists, and playlists
- Download tracks with yt-dlp
- Convert audio to AAC via ffmpeg
- Embed metadata (title, artist, album, cover art) with mutagen
- Upload finished files to S3-compatible storage
- Track job status via Celery result backend

## Project Structure

```
zimmporter-api/
├── zimmporter/          # Main application package
│   ├── main.py          # FastAPI app entry point
│   ├── models/          # SQLAlchemy models
│   ├── routers/         # API route handlers
│   ├── schemas/         # Pydantic request/response schemas
│   ├── tasks/           # Celery task definitions
│   └── utils/           # Shared utilities
├── tests/               # Test suite
├── Dockerfile           # API server image
├── Dockerfile.worker    # Celery worker image
└── pyproject.toml       # Project configuration
```
