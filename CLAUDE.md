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

Pushing to `main` deploys the site via **Cloudflare Pages** (Workers static assets, Git-connected). Build command is empty (no build step); assets are served from the repo root. The production URL is configured in the Cloudflare dashboard.

A `.assetsignore` file at the repo root excludes `.git`, `.github`, `CLAUDE.md`, and `.DS_Store` from the deployed assets.
