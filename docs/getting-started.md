# Getting Started

## Prerequisites

- Python 3.14+
- Node.js 20+
- Docker and Docker Compose (for local dev)
- MariaDB (or use the provided Docker setup)
- Valkey / Redis (for Celery broker)
- S3-compatible storage (MinIO for local dev)

## Quick Start with Docker Compose

Clone the API and frontend repositories:

```bash
git clone https://github.com/Tomifarmer/zimmporter-api.git
git clone https://github.com/Tomifarmer/zimmporter-front.git
```

Start the stack:

```bash
cd zimmporter-api
docker compose up -d
```

This starts the API, Celery worker, MariaDB, Valkey, and connects to an external S3-compatible store.

Install and run the frontend:

```bash
cd zimmporter-front
npm ci
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

## Environment Variables

The API requires the following environment variables:

| Variable | Description |
|---|---|
| `DB_HOST` / `DB_PORT` / `DB_USER` / `DB_PASS` / `DB_NAME` | MariaDB connection details |
| `CELERY_BROKER` / `CELERY_BACKEND` | Valkey/Redis URLs |
| `AWS_ENDPOINT_URL` | S3-compatible endpoint |
| `AWS_ACCESS_KEY_ID` | S3 access key |
| `AWS_SECRET_ACCESS_KEY` | S3 secret key |
| `AWS_BUCKET` | Target bucket name |

See the [API configuration](api/configuration.md) page for all available variables.
