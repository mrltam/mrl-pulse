# MRL Pulse — Live Test Availability

A public-facing real-time dashboard displaying which laboratory tests are currently available at the **Madinah Regional Laboratory**. Designed for referring hospitals to verify availability before drawing samples.

---

## Project structure

```
mrl_pulse/
├── index.html              # Main site (HTML + CSS + JS, single file)
├── robots.txt              # Blocks all search engines and archivers
├── assets/
│   └── logos/
│       ├── cluster.png     # Madinah Health Cluster logo
│       └── mrl.png         # Regional Laboratory logo
└── data/
    ├── tests.json          # Static catalogue: 414 tests, 12 panels, 7 departments
    └── availability.json   # Dynamic state: which tests/panels are ON/OFF
```

---

## Local preview

Open a terminal in the `mrl_pulse/` folder and run:

```bash
python -m http.server 8000
```

Then open **http://localhost:8000** in your browser.

> The site fetches `data/tests.json` and `data/availability.json`, so you must serve it via HTTP — opening `index.html` directly with `file://` will fail due to browser security.

---

## Deploying to GitHub Pages

1. Create a new **public** repository on GitHub (e.g., `mrl-pulse`).
2. Push these files to the `main` branch.
3. Go to **Settings → Pages**.
4. Under "Source", select **Deploy from a branch** → `main` → `/ (root)` → Save.
5. Wait ~1 minute. Your site will be live at:
   `https://<username>.github.io/mrl-pulse/`

---

## How availability updates work

The desktop control app (built next) updates `data/availability.json` via the GitHub API. The site auto-refreshes that file every **30 seconds** in the background.

### `availability.json` shape

```json
{
  "tests":   { "<test_id>": true | false, ... },
  "panels":  { "<panel_id>": true | false, ... },
  "maintenance_mode": false,
  "last_updated": "2026-01-15T08:00:00Z",
  "updated_by": "TAM"
}
```

### Behavior

- **`maintenance_mode: true`** → entire site is replaced with a "Service temporarily unavailable" overlay.
- **Panel master OFF** → all sub-tests under that panel are dimmed/red, regardless of their individual flag.
- **Test individual OFF** → that single test is dimmed/red.
- **Missing key in `tests` or `panels`** → defaults to ON (available).

---

## Privacy & visibility

- **No search engine indexing**: `<meta name="robots" content="noindex,nofollow,noarchive">` + `robots.txt`.
- **No tracking**: zero analytics, zero cookies.
- **No patient data**: only the public test catalogue.
- **Easy shutdown**: see the next section.

---

## Killing the site (3 ways, fastest first)

1. **Maintenance mode** (10 seconds) — flip `maintenance_mode: true` in the desktop app. Site goes blank instantly. Reversible.
2. **Unpublish GitHub Pages** (1 minute) — Settings → Pages → Unpublish. Site returns 404. Repo intact, reversible.
3. **Delete repository** (2 minutes) — Settings → Delete this repository. Permanent.

---

## External dependencies

- **Google Fonts**: Bricolage Grotesque, Manrope, JetBrains Mono — loaded from fonts.googleapis.com
- **QR generator**: `api.qrserver.com` — used in the footer to point to the current page URL

Both are optional; the site degrades gracefully if blocked.

---

## Next phase

The desktop control app (`MRL_Pulse_Control.py`) — built with `customtkinter` — will:
- Authenticate users (TAM = Super Admin, others restricted by department)
- Render tabbed views per department with toggles
- Master toggles for the 12 panels
- Maintenance mode kill-switch
- User & permission management (Super Admin only)
- Push changes to `availability.json` via GitHub API with optimistic locking
