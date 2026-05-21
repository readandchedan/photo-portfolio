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
2. Copy `series/sample/index.html` to `series/<name>/index.html` and update:
   - `<title>`, series heading, description
   - `src`/`srcset` paths pointing to `images/...`
   - `alt` text for each photo
3. Add a card to `index.html` inside `.series-grid` linking to the new series. Use `loading="lazy"` on images.

## Architecture Notes

- All pages share `css/style.css` and `js/main.js` via relative paths (`../../css/style.css` from series pages).
- Series pages include a lightbox container (`#lightbox`) that is toggled via `main.js`. Gallery images call `openLightbox(this)` on click.
- Images use `srcset` for responsive sizing; the CSS gallery grid is `repeat(auto-fill, minmax(350px, 1fr))`.
- The homepage series grid uses `aspect-ratio: 4 / 3` for card thumbnails.
