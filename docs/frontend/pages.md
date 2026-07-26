# Pages

## Home (`/`)

Dashboard with system health cards (API, Redis, Celery, MariaDB), job overview stats, and recent jobs list. Auto-refreshes health every 10s, jobs every 5s.

## Search (`/search`)

The main interface for searching YouTube Music. Supports search by album and playlist.

- Search input with type selector (albums / playlists) and result limit
- Results displayed as cards with album art
- Multi-select results with checkboxes
- Concurrent download slider (1–32, default 4)
- Select All toggle button
- Batch download button to start downloads for all selected items

After starting a download, redirects to the job detail page.

## Job List (`/jobs`)

Displays all import jobs with pagination (20 per page):

- **Pending** — waiting for a worker
- **Running** — actively being processed
- **Success** — successfully uploaded to S3
- **Failed** — error during processing

Each row shows job ID, type (album/playlist), progress bar, status badge, timestamps. Click to expand message and error details. Table auto-refreshes every 5s.

## Job Detail (`/jobs/[id]`)

Detailed view of a single job with:

- Real-time polling (every 3 seconds) while job is pending/running
- Progress summary (current album, album progress, song counts)
- Per-song status table with download status for each track
- Retry button for failed songs within the job
