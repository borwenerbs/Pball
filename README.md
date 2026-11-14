# Pickleball Scorekeeper v2

## Included
- Installable offline-first PWA; local records stored in IndexedDB.
- Bulk player entry (comma or line-break separated).
- Doubles team selection with visually distinct Team A and Team B panels.
- Play-for-serve deciding point before match start.
- Large shouted score call, centered score display, Home / Undo / Away rally row.
- Player win-loss records, percentage ranking, attendance filter based on the current match roster.
- Light / Dark / System theme preference.
- JSON export/import.
- Optional ZimaOS sync API service (basic shared JSON snapshot endpoint).

## Deploy the PWA
Upload the contents of `pwa/` to any HTTPS static host (Netlify, Cloudflare Pages, or your own reverse proxy). HTTPS is required for service workers and installation. Open the HTTPS address in Safari on iPhone, tap Share, then Add to Home Screen. Open the installed app once while online so its files are cached; after that, scoring and local records work offline.

## ZimaOS sync server (optional)
The `server/` folder includes a small Node.js service and Dockerfile. In ZimaOS, deploy it as a Docker Compose project using the included `docker-compose.yml`, or build the Dockerfile. It listens on port 8080. Put it behind HTTPS using your reverse proxy before exposing it outside your LAN. In the app's Settings, enter the base URL, e.g. `https://score-sync.yourdomain.example`, then Save Server Address and Sync Now.

**Important:** The included sync service is a simple single-snapshot prototype with no user authentication, encryption layer of its own, or sophisticated conflict resolution. Do not expose it to the public internet without adding authentication and access controls. For initial testing, keep it on your trusted local network or behind an authenticated reverse proxy. Export JSON backups regularly.

## Scoring note
The prototype implements the standard doubles two-server sequence at the team level: server 1, then server 2, then side-out. It uses the initial play-for-serve winner as the first serving team. The opening score call is `0-0-2`, following the common doubles opening convention. Individual player court-position rotation is not yet tracked; the server-number call is tracked.

## Current limitations
- No cloud authentication or encrypted transport beyond HTTPS supplied by your deployment.
- Sync currently merges players and completed matches by ID; it does not yet reconcile concurrent edits to the same player record.
- Attendance ranking currently uses the players in the active match roster as the attendance set.
- Test scoring rules against your league's adopted rulebook before relying on it for official tournament play.
