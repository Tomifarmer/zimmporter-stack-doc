# Frontend Configuration

## Environment Variables

Set in `.env.local` for local development, or via container environment at runtime.

| Variable | Default | Description |
|---|---|---|
| `API_URL` | `http://localhost:8000` | Base URL of the Zimmporter API |
| `API_KEY` | `""` | API key sent as `X-API-Key` header when `USE_SIMPLE_AUTH=true` |
| `USE_SOCIAL_LOGIN` | `false` | Enable social login via NextAuth (OIDC/GitHub); proxy redirects to `/login` |
| `USE_SIMPLE_AUTH` | `false` | Enable API key auth; sends `X-API-Key` header to backend |
| `OIDC_NAME` | `"OIDC"` | Display name for the OIDC provider on the login button |
| `OIDC_ISSUER_URL` | `""` | OIDC issuer URL (e.g. `https://accounts.google.com`) |
| `OIDC_CLIENT_ID` | `""` | OIDC client ID |
| `OIDC_CLIENT_SECRET` | `""` | OIDC client secret |
| `GITHUB_CLIENT_ID` | `""` | GitHub OAuth App client ID |
| `GITHUB_CLIENT_SECRET` | `""` | GitHub OAuth App client secret |
| `AUTH_SECRET` | `"dev-secret-change-in-production"` | NextAuth encryption secret (generate with `openssl rand -base64 32`) |

`USE_SOCIAL_LOGIN` and `USE_SIMPLE_AUTH` cannot both be `true`; the app shows an error overlay if both are enabled.

If `API_URL` is set to a value that does not start with `http://` or `https://`, the server logs an error and exits with code 1 at startup.

The config is injected server-side into `window.__RUNTIME_CONFIG__` at page render time and accessed client-side via `lib/config.ts`.

## Scripts

| Command | Description |
|---|---|
| `npm run dev` | Start development server (port 3000) |
| `npm run build` | Production build (standalone output) |
| `npm run lint` | Biome check (lint + format + imports) |
| `npm run format` | Biome format --write |
| `npm run lint:fix` | Biome check --write |
| `npm test` | Vitest (single run) |
| `npm run test:watch` | Vitest (watch mode) |

## Docker

```bash
docker build -t zimmporter-front .
docker run -p 3000:3000 -e API_URL=http://localhost:8000 zimmporter-front
```
