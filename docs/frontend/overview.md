# Frontend Overview

The Zimmporter frontend is a **Next.js 16** application with **React 19** that provides a web UI for searching YouTube Music, creating import jobs, and tracking progress.

## Stack

- **Framework:** Next.js 16 (App Router)
- **UI:** React 19, PrimeReact 10, react-bootstrap / bootstrap 5
- **State / Data:** TanStack React Query 5, Axios
- **Testing:** Vitest, React Testing Library, jsdom
- **Linting:** ESLint (Next.js config)

## Project Structure

```
zimmporter-front/
├── src/
│   ├── app/           # Next.js App Router pages
│   ├── components/    # Reusable UI components
│   ├── api/           # API client
│   └── hooks/         # Custom React hooks
├── public/            # Static assets
├── Dockerfile         # Container image
└── package.json       # Dependencies and scripts
```
