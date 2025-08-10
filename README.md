# Vue Weather App

A lean, client-side weather app using **Vue 3 (CDN)**, **Open-Meteo** (forecast + geocoding), and **BigDataCloud** (reverse geocoding). No build step. Just run a local server and go.

## Features

- **Type-ahead search** (fires at 3+ chars) with clean labels (`filter(Boolean).join(', ')`).
- **Outside-click to close** the results (document-level `pointerdown` + `ref.contains`).
- **Abort in-flight fetches** with `AbortController` to prevent stale overwrites.
- **Use My Location** via the Geolocation API (with error handling and timeout).
- **Forecasts:** current, hourly (next 5 hrs), and daily (min/max + sunrise/sunset).
- **Dark/Light theme** with class-scoped CSS variables and **favicon swap**.
- **Responsive** dropdown widths via media queries.
- Tiny, dependency-free (just Vue via CDN).

## Tech & APIs

- **Vue 3** (Composition API) via CDN — `ref`, `watch`, `onMounted`, `onBeforeUnmount`
- **Open-Meteo Forecast API** — no key
- **Open-Meteo Geocoding API** — no key
- **BigDataCloud Reverse Geocoding** — no key
- WMO weather codes → emoji/text map

## File Structure

```
/ (project root)
├─ index.html          # App markup + Vue logic (inline <script>)
├─ styles/
│  └─ styles.css       # Theme, layout, dropdown styles
├─ fav-dark.ico
└─ fav-light.ico
```

## Run Locally

> Opening `file://` directly can break geolocation/CORS. Use a local server.

Pick one:

- VS Code: “Live Server” → Right-click `index.html` → *Open with Live Server*
- Node: `npx serve`  (or `npx http-server`)
- PHP: `php -S 127.0.0.1:8080`
- Python 3: `python3 -m http.server 8080`

Then visit: `http://localhost:8080`

## How It Works

- **Search flow**
    - `query` (user input) → `watch(query)` gates at **length ≥ 3** → sets `searchQuery`.
    - `watch(searchQuery)` calls Open-Meteo **geocoding**.
    - Results go to `searchResults`; dropdown shows via `v-if="showSearchResults"`.

- **Close on outside click**
    - A document-level `pointerdown` listener checks `!searchWrap.contains(e.target)` and sets `showSearchResults = false`. (Works on mobile; capture phase enabled.)

- **Select a result**
    - `fetchWeatherBySearchTerm(result)` sets `latitude/longitude`, closes dropdown, then fetches forecast.

- **Use My Location**
    - `getLocationAndWeather()` → Geolocation → forecast → BigDataCloud reverse-geocode for `city`.

- **Network hygiene**
    - Before geocoding: `controller?.abort()` → new `AbortController()` to kill stale requests.

## Theme Knobs (Dropdown Colors)

Theme-scoped CSS variables already wired; tweak these in your CSS:

```css
/* Dark */
body.dark-mode {
  --search-bg:            #111b2e;
  --search-border:        #2a3645;
  --search-shadow:        0 12px 24px rgba(0,0,0,.45);
  --search-item-hover-bg: rgba(106,125,255,.14);
  --search-separator:     1px solid rgba(232,238,246,.06);
  --search-text:          var(--font-color);
}

/* Light */
body.light-mode {
  --search-bg:            #ffffff;
  --search-border:        #d6dfea;
  --search-shadow:        0 12px 24px rgba(15,23,42,.10);
  --search-item-hover-bg: rgba(76,111,255,.12);
  --search-separator:     1px solid rgba(15,23,42,.06);
  --search-text:          var(--font-color);
}
```

## Accessibility & UX Notes

- Dropdown is keyboard-friendly once you add focus styles and key handlers (nice future task).
- `pointerdown` closes earlier than `click`, reducing “ghost taps” on mobile.

## Roadmap

- °C/°F toggle (convert or request unit-specific data).
- Debounce search (e.g., 250 ms) to cut API calls while typing.
- Cache last successful results in `sessionStorage`.
- Keyboard navigation for results (↑/↓/Enter/Escape).
- Precipitation/UV in the hourly strip.

## License

MIT. Use it, tweak it, ship it.