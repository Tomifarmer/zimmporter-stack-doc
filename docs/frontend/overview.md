# Frontend Overview

The Zimmporter frontend is a **Next.js** application with **React** that provides a web UI for searching YouTube Music, creating import jobs, and tracking progress.

## Stack

- **Framework:** Next.js (App Router, standalone output)
- **UI:** React, PrimeReact, Bootstrap 5, PrimeIcons
- **State / Data:** TanStack React Query, Axios
- **Testing:** Vitest, React Testing Library, jsdom
- **Linting:** ESLint (Next.js config)

## Project Structure

```
zimmporter-front/
├── src/
│   ├── app/               # Next.js App Router pages
│   │   ├── page.tsx       # Redirects to /search
│   │   ├── search/        # Search and download page
│   │   ├── jobs/          # Job list and detail pages
│   │   ├── layout.tsx     # Root layout, providers, config injection
│   │   ├── not-found.tsx  # Custom 404
│   │   └── globals.css    # Global styles, PrimeReact + Bootstrap
│   ├── components/        # Reusable UI components
│   │   ├── Header/        # Navigation header with health check
│   │   ├── Footer/        # Version info footer
│   │   ├── JobRow/        # Job list item component
│   │   ├── StatusBadge/   # Status indicator badge
│   │   ├── PageContainer/ # Content layout wrapper
│   │   ├── Lightfall/     # WebGL animated background
│   │   └── LightfallBackground/ # Background wrapper component
│   ├── lib/               # API client (Axios) and runtime config
│   ├── hooks/             # Custom React hooks (useJobPolling)
│   ├── providers/         # TanStack Query provider
│   ├── types/             # TypeScript interfaces (SearchResult, JobStatusResponse, Song, etc.)
│   └── config/            # Version info
├── tests/                 # Vitest test suite
├── public/                # Static assets
├── Dockerfile             # Container image (multi-stage, standalone)
└── package.json           # Dependencies and scripts
```
