# Pages

## Home (`/`)

Dashboard with system health cards (API, Redis, Celery, MariaDB), job overview stats, and recent jobs list. Auto-refreshes health every 10s, jobs every 5s.

## Search (`/search`)

The main interface for searching YouTube Music. Supports search by album, featured playlist, or community playlist.

- Search input with type selector (albums / featured playlists / community playlists) and result limit
- Search input is auto-focused on page load; typing anywhere on the page (when not focused on another input) starts or refines the query
- Results displayed as cards with album art (loaded as base64 data URIs when `API_PROXY_FETCH=true`, no separate image requests)
- Green checkmark badge on covers of albums/playlists already in the S3 library (`available` flag from the API)
- Multi-select results with checkboxes
- Concurrent download slider (1–32, default 4)
- Select All toggle button
- Batch download button to start downloads for all selected items

After starting a download, redirects to the job detail page.

## Job List (`/jobs`)

Displays all import jobs with pagination (20 per page):

- A single interactive toolbar of status pills (Total / Running / Completed / Partial / Failed) — clicking a pill filters the table, each pill shows its live count
- **Pending** — waiting for a worker
- **Running** — actively being processed
- **Success** — successfully uploaded to S3
- **Failed** — error during processing

Each row shows job ID, type (album/playlist), progress bar, status badge, timestamps. Click to expand message and error details. Table auto-refreshes every 5s.

## Settings (`/settings`)

Manage the YouTube cookies used for age-restricted downloads:

- Cookie status badge (Configured / Not configured / Stale), cookie count, domains, and last-updated timestamp
- Upload a Netscape-format cookies file (`.txt`, `.cookies`, `.tidycookies`) via `POST /cookies`; errors are shown inline
- When the backend flags the cookies as stale, an amber warning banner is shown at the top of every page with a link to this page

## Job Detail (`/jobs/[id]`)

Detailed view of a single job with:

- Real-time polling (every 3 seconds) while job is pending/running
- Progress summary (current album, album progress, song counts)
- Per-song status table with download status for each track
- Retry button for failed songs within the job
