# velov-ebikes

Single-file page (index.html, no build, no dependencies) showing Vélo'v
stations with available e-bikes near the user's location in Lyon.
Live at https://ahmad.shahwan.pw/velov-ebikes/ via GitHub Pages
(.github/workflows/pages.yml deploys on every push to main).

## How it works
- Reads the Grand Lyon GBFS feed: gbfs.json → station_information (fetched
  once) + station_status, fetched every 5/10/20 s or "update" (sync),
  picked from the "Every" menu and saved; default 20 s; `?every=` overrides
  (seconds, minimum 5, or `sync`). Sync schedules each fetch at
  last_updated + ttl (60 s if absent) + a margin, retrying after
  5/10/15/20 s while the data is still old. The margin starts at 2 s and
  adapts: +2 s when an on-time fetch finds old data, −0.25 s per on-time hit. Polling uses a setTimeout chain; the
  countdown ring sweeps over the actual wait.
- The header shows data freshness from station_status `last_updated`
  ("data from 45 s ago"), not time since fetch; it falls back to fetch time
  ("fetched …") if the feed omits it. Tapping it refetches and restarts the
  countdown.
- Polling and GPS (watchPosition) run only while the page is visible;
  both stop on visibilitychange.
- E-bike count: `num_ebikes_available` if present, else sums
  `vehicle_types_available` whose type isn't `propulsion_type: human`.
- The feed allows cross-origin requests, so no proxy is needed
  (`?proxy=` exists as a fallback).
- Two layouts, toggled and saved in localStorage: map (default) and list
  (station rows by distance). Both share the same header and controls (no
  layout shift on toggle); the nearest open station with an e-bike is
  emphasised in each, and pulses + vibrates when one appears after none. The map is hand-rolled, not a library: a
  Web Mercator view centred on the user, scaled so the selected radius fits
  (the radius picker acts as zoom). Station markers render immediately;
  OpenStreetMap raster tiles (no API key; muted via CSS filter, inverted in
  dark mode) load in the background and fade in, and rendering never waits
  for them. Keep the OSM attribution visible. Tapping a marker opens a detail card.

## Conventions
- Keep it one self-contained file, vanilla JS, minimal dependencies.
- Build DOM with textContent (station names are external data).
- Light/dark via CSS variables on :root; mobile-first.
- Radius and refresh interval are compact drop-down pickers ("Within 500 m ▾",
  "Every 20 s ▾") overlaying the content briefly; keep the controls to one row.
- Test against a mock GBFS feed before pushing.
