# Pages

## Search Page

The main interface for searching YouTube Music. Supports search by album, artist, and playlist.

- Search input with autocomplete
- Results displayed as cards with album art
- One-click import button on each result

## Job Queue

Displays all import jobs with their current status:

- **Pending** — waiting for a worker
- **Running** — actively being processed
- **Completed** — successfully uploaded to S3
- **Failed** — error during processing

Each job shows progress, elapsed time, and links to the imported tracks.

## Track Library

Browse all imported tracks with search, filter, and sort capabilities.

- Grid and list views
- Filter by album, artist, or date
- Direct S3 download links

## Settings

Configuration page for S3 endpoint, API URL, and other user preferences (optional, depending on deployment).
