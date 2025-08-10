# Vue Weather App

Free to use.

## What I built
- City search with a 3+ character gate using Vue 3 watchers.
- Geocoding via Open‑Meteo (no key); renders a dropdown of matches.
- Outside‑click to close the dropdown using a document `pointerdown` listener + `ref.contains` (mobile‑safe).
- Aborts in‑flight geocoding requests with `AbortController` to prevent stale results.
- “Use My Location” via the Geolocation API with error handling and timeouts.
- Weather fetch from Open‑Meteo: current, hourly (next hours), and daily (min/max, sunrise, sunset).
- WMO weather code → emoji/text mapping for readable conditions.
- Reverse geocoding to a city name with BigDataCloud.
- Dark/Light theme toggle with favicon swap and class‑based theming.
- Loading and error states surfaced in the UI.
- Simple, responsive layout.

## Run
Use a local server (recommended for geolocation & CORS). Examples:
- VS Code Live Server
- Node: `npx serve` or `npx http-server`
- Python: `python3 -m http.server 8080`

Open the served URL (e.g., `http://localhost:8080`). No build step required.