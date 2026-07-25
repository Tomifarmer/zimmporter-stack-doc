# Frontend Configuration

## Environment Variables

| Variable | Required | Default | Description |
|---|---|---|---|
| `NEXT_PUBLIC_API_URL` | Yes | — | Base URL of the Zimmporter API |
| `NEXT_PUBLIC_S3_ENDPOINT` | No | — | S3 endpoint for direct downloads |

## Scripts

| Command | Description |
|---|---|
| `npm run dev` | Start development server (port 3000) |
| `npm run build` | Production build |
| `npm run start` | Start production server |
| `npm run lint` | Run ESLint |
| `npm test` | Run Vitest tests |

## Docker

```bash
docker build -t zimmporter-front .
docker run -p 3000:3000 -e NEXT_PUBLIC_API_URL=http://localhost:8000 zimmporter-front
```
