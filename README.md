# Vélo'v e-bikes nearby

A single-page utility that shows Vélo'v stations with available electric bikes around your current location in Lyon.
It polls the open GBFS feed published by Métropole de Lyon / JCDecaux every 20 seconds, only while the page is visible.

Live page: https://ahmadshahwan.github.io/velov-ebikes/

## Query parameters

- `every=30` sets the polling interval in seconds (minimum 10).
- `gbfs=<url>` points to another GBFS auto-discovery feed.
- `proxy=<prefix>` rewrites the feed origin through a same-origin proxy, for when the feed blocks cross-origin requests.

Data: Métropole de Lyon / JCDecaux, Licence Ouverte 2.0.
