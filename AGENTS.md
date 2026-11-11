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

## Conventions
- Keep it one self-contained file, vanilla JS, minimal dependencies.
- Build DOM with textContent (station names are external data).
- Light/dark via CSS variables on :root; mobile-first.
- Test against a mock GBFS feed before pushing.
