# Frontend Overview

The Zimmporter frontend is a **Next.js** application with **React** that provides a web UI for searching YouTube Music, creating import jobs, and tracking progress.

## Stack

- **Framework:** Next.js 16 (App Router, standalone output)
- **UI:** React 19, PrimeReact, Bootstrap 5, PrimeIcons
- **State / Data:** TanStack React Query, Axios
- **Auth:** NextAuth v5 (OIDC + GitHub providers)
- **Testing:** Vitest, React Testing Library, jsdom
- **Linting / Formatting:** Biome

## Project Structure

```
zimmporter-front/
├── src/
│   ├── proxy.ts            # Next.js middleware — auth redirect
│   ├── app/
│   │   ├── (app)/          # Authenticated app pages (layout + pages)
│   │   ├── (auth)/         # Auth pages (login)
│   │   ├── api/            # API routes (config, nextauth)
│   │   ├── layout.tsx      # Root layout
│   │   ├── not-found.tsx   # Custom 404
│   │   └── globals.css     # Global styles
│   ├── components/
│   │   ├── Header/         # Navigation header (brand, nav links, health dots, avatar)
│   │   ├── Footer/         # Version info footer
│   │   ├── ApiKeyErrorOverlay.tsx
│   │   ├── AuthConflictOverlay.tsx
│   │   ├── SocialLoginErrorOverlay.tsx
│   │   └── ...
│   ├── lib/
│   │   ├── api.ts          # Axios client with auth interceptors
│   │   ├── auth.ts         # NextAuth v5 config
│   │   └── config.ts       # Runtime config (useSocialLogin, apiUrl, apiKey)
│   ├── hooks/              # useJobPolling
│   ├── providers/          # auth-provider, query-provider
│   ├── types/              # TypeScript interfaces + next-auth.d.ts
│   └── __tests__/          # Vitest test suite
├── Dockerfile              # Multi-stage standalone image
└── package.json
```

## Key Features

- Search YouTube Music for albums and playlists
- Multi-select results with batch download
- Real-time job progress via polling
- Optional social login (OIDC / GitHub) or API key auth
- Auth overlays guide users when configuration is missing
