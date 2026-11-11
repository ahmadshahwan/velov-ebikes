# velov-ebikes

Single-file page (index.html, no build, no dependencies) showing Vélo'v
stations with available e-bikes near the user's location in Lyon.
Live at https://ahmad.shahwan.pw/velov-ebikes/ via GitHub Pages
(.github/workflows/pages.yml deploys on every push to main).

## How it works
- Reads the Grand Lyon GBFS feed: gbfs.json → station_information (fetched
  once) + station_status (polled every 20 s, `?every=` overrides).
- Polling and GPS (watchPosition) run only while the page is visible;
  both stop on visibilitychange.
- E-bike count: `num_ebikes_available` if present, else sums
  `vehicle_types_available` whose type isn't `propulsion_type: human`.
- The feed allows cross-origin requests, so no proxy is needed
  (`?proxy=` exists as a fallback).
- Two layouts, toggled and saved in localStorage: map (default) and list
  (battery hero + station rows). The map is hand-rolled, not a library: a
  Web Mercator view centred on the user, scaled so the selected radius fits
  (the radius buttons act as zoom). Station markers render immediately;
  OpenStreetMap raster tiles (no API key; muted via CSS filter, inverted in
  dark mode) load in the background and fade in, and rendering never waits
  for them. Keep the OSM attribution visible. Tapping a marker opens a detail card.

## Conventions
- Keep it one self-contained file, vanilla JS, minimal dependencies.
- Build DOM with textContent (station names are external data).
- Light/dark via CSS variables on :root; mobile-first.
- Test against a mock GBFS feed before pushing.
