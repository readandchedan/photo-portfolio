# Photo Portfolio

A static photography portfolio site. No build step, no dependencies — just HTML, CSS, and images.

## Running locally

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Adding a photo series

1. Create `series/<name>/images/` and place your photos there.
2. Copy `series/black-and-white/index.html` to `series/<name>/index.html` and edit:
   - `<title>`, series heading, description
   - `src`/`srcset` paths pointing to `images/...`
   - `alt` text for each photo
3. Add a card to `index.html` inside `.series-grid` linking to the new series.

## Structure

- `index.html` — Homepage with series cards
- `about.html` — About page
- `css/style.css` — Shared stylesheet
- `js/main.js` — Lightbox
- `series/<name>/index.html` — Gallery page
- `series/<name>/images/` — Photo assets

## Notes

- Galleries use a masonry layout (CSS columns). Images flow top-to-bottom within each column.
- Homepage thumbnails are cropped to a 4:3 ratio. Use a horizontal image to avoid cropping.
