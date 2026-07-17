# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static HTML/CSS/JS photography portfolio site. There is no build system, no package manager, and no test suite. All files are served as-is.

## Development

Preview the site with any static file server:

```bash
python3 -m http.server 8000
# or
npx serve .
```

Then open `http://localhost:8000`.

## Site Structure

- `index.html` — Homepage with a responsive grid of photo series cards (`series-grid`).
- `about.html` — Static about page.
- `css/style.css` — Single shared stylesheet for all pages.
- `js/main.js` — Lightbox functionality (`openLightbox`, `closeLightbox`, Escape key handler).
- `series/<name>/index.html` — Individual series gallery page.
- `series/<name>/images/` — Photo assets for that series.

## Adding a New Photo Series

1. Create `series/<name>/images/` and place photos there.
2. Copy `series/black-and-white/index.html` to `series/<name>/index.html` and update:
   - `<title>`, series heading, description
   - `src`/`srcset` paths pointing to `images/...`
   - `alt` text for each photo
3. Add a card to `index.html` inside `.series-grid` linking to the new series. Use `loading="lazy"` on images.

## Architecture Notes

- All pages share `css/style.css` and `js/main.js` via relative paths (`../../css/style.css` from series pages).
- Series pages include a lightbox container (`#lightbox`) that is toggled via `main.js`. Gallery images call `openLightbox(this)` on click.
- The gallery uses a CSS columns masonry layout (`column-count: 3` on desktop, `2` on tablet, `1` on mobile). Images flow **top-to-bottom within each column**, not left-to-right across rows. `break-inside: avoid` prevents images from splitting across columns.
- The homepage series grid uses `aspect-ratio: 4 / 3` with `object-fit: cover` for card thumbnails — choose a horizontal image to avoid cropping.

## Deployment

Pushing to `main` triggers `.github/workflows/deploy.yml`, which streams a tar archive over SSH to the VPS (mirrors `../vps-tools/deploy-photo-portfolio.sh`). The remote directory is wiped and replaced on each deploy. Excludes `.git`, `.github`, and `CLAUDE.md`.

Required repository secrets (**Settings → Secrets and variables → Actions**):

- `VPS_SSH_KEY` — private key for `oscar@208.87.133.186` (ed25519 recommended; the matching public key must already be authorized on the VPS).
- `VPS_HOST` — `208.87.133.186`
- `VPS_USER` — `oscar`
- `VPS_DIR` — `/var/www/photo-portfolio`
- `VPS_KNOWN_HOSTS` *(recommended)* — output of `ssh-keyscan -H 208.87.133.186`. If omitted, the workflow falls back to `ssh-keyscan` at runtime (less secure against MITM).

To generate a dedicated deploy key:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/photo_portfolio_deploy -C "github-actions:photo-portfolio"
ssh-copy-id -i ~/.ssh/photo_portfolio_deploy.pub oscar@208.87.133.186
# paste ~/.ssh/photo_portfolio_deploy into VPS_SSH_KEY
ssh-keyscan -H 208.87.133.186   # paste into VPS_KNOWN_HOSTS
```

The `workflow_dispatch` trigger also allows manual runs from the Actions tab.
