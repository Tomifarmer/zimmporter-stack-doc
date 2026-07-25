# Frontend Configuration

## Environment Variables

| Variable | Required | Default | Description |
|---|---|---|---|
| `API_URL` | No | `http://localhost:8000` | Base URL of the Zimmporter API |
| `API_KEY` | No | — | API key for `X-API-Key` header auth |

The config is injected server-side into `window.__RUNTIME_CONFIG__` at page render time and accessed client-side via `lib/config.ts`.

## Scripts

| Command | Description |
|---|---|
| `npm run dev` | Start development server (port 3000) |
| `npm run build` | Production build (standalone output) |
| `npm run start` | Start production server |
| `npm run lint` | Run ESLint |
| `npm test` | Run Vitest tests |

## Docker Compose

```yaml
services:
  frontend:
    build: .
    ports:
      - "3000:3000"
    environment:
      - API_URL=http://localhost:8000
      - API_KEY=
```

## Docker

```bash
docker build -t zimmporter-front .
docker run -p 3000:3000 -e API_URL=http://localhost:8000 zimmporter-front
```
