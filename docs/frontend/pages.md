# Pages

## Home (`/`)

Redirects to `/search`.

## Search (`/search`)

The main interface for searching YouTube Music. Supports search by album, artist, and playlist.

- Search input with type selector (album / artist / playlist) and result limit
- Results displayed as cards with album art
- Multi-select results with checkboxes
- Concurrent download slider (1–8 parallel downloads)
- Batch import button to start downloads for all selected items

After starting a download, redirects to the jobs list page.

## Job List (`/jobs`)

Displays all import jobs with pagination (limit/offset):

- **Pending** — waiting for a worker
- **Running** — actively being processed
- **Success** — successfully uploaded to S3
- **Failed** — error during processing

Each job shows type (album/playlist), status badge, progress (album and song counts), error messages, and timestamps. Table auto-refreshes periodically.

## Job Detail (`/jobs/[id]`)

Detailed view of a single job with:

- Real-time polling (every 3 seconds) while job is pending/running
- Progress summary (current album, album progress, song counts)
- Per-song status table with download status for each track
- Retry button for failed songs within the job
