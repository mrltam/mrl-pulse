# MRL Pulse — Public Site

Live test availability display for the Madinah Regional Laboratory.

## Files

- `index.html` — Main site (single file, no build step)
- `robots.txt` — Prevents search engine indexing
- `data/tests.json` — Test catalogue (managed by control app)
- `data/availability.json` — Live availability state (managed by control app)
- `assets/logos/` — Cluster + MRL logo PNGs

## Cache busting

The site auto-refreshes data every 20 seconds. Each fetch uses a fresh random query parameter and the `cache: 'no-store'` directive to bypass any CDN/browser cache.

## Live URL

https://mrltam.github.io/mrl-pulse/

## Updating availability

Use the **MRL Pulse Control** desktop app to toggle test availability. Changes are committed to GitHub and reflected on the public site within ~30 seconds.
