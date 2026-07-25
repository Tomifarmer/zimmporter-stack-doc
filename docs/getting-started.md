# Getting Started

## Prerequisites

- Python 3.14+
- Node.js 20+
- Docker and Docker Compose (for local dev)
- MariaDB (or use the provided Docker setup)
- Redis / Valkey (for Celery broker)
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

This starts the API, Celery worker, MariaDB, Redis, and MinIO.

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
| `DATABASE_URL` | MariaDB connection string |
| `CELERY_BROKER_URL` | Redis/Valkey URL |
| `S3_ENDPOINT` | S3-compatible endpoint |
| `S3_ACCESS_KEY` | S3 access key |
| `S3_SECRET_KEY` | S3 secret key |
| `S3_BUCKET` | Target bucket name |

See the [API configuration](api/configuration.md) page for details.
