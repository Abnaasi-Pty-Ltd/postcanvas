# JP Carousel Studio v4

A local-first LinkedIn carousel post studio that runs in Chrome as a single HTML file. Outputs 1200x1200px PNG slides ready for LinkedIn document post upload.

## Quick Start

1. Open `index.html` in Chrome
2. Edit slide content in the left panel
3. Click "Export All PNGs" to download slides

## Features

- **17 content block types** — sub-text, body text, stat rows, bullet boxes, numbered steps, code blocks, comparison columns, grid cards, CTA boxes, quotes, tag pills, images, highlight bars, flow diagrams, and more
- **Modular block system** — add, remove, and reorder blocks freely on each slide (up to 4 per slide)
- **14 themes** — Dark Editorial, Warm Cream, Deep Navy, Chalk, Pitch Black, Luxury Gold, Saffron, Forest, Linen Tutorial, Linen Minimal, Magazine, Breaking News, Tech Dark, Burgundy
- **10 background styles** — plain, grid, dots, 3D shapes, circles, gradient, glow, multi-glow, diagonal lines, noise
- **Gradient/glow builder** — 3 colour pickers, opacity slider, direction control, 8 preset glow swatches
- **8 headline fonts** — Playfair Display, Fraunces, Bebas Neue, Syne, Outfit, DM Serif Display, Inter, Space Grotesk
- **Per-slide theme override** — each slide can use the global theme or its own
- **Drag to reorder** slides via tabs
- **Image upload** — base64 encoded, renders in canvas, exports correctly
- **PNG export** — html2canvas at 2.6x scale (~1200x1200px output)
- **Draft system** — save/load from localStorage, export/import as JSON
- **Dark/light mode** for the studio UI
- **Keyboard shortcuts** — arrows navigate, 1-9 jump, S save, D dark/light, E export

## Deployment

Deploy to GitHub Pages:

1. Push this repo to GitHub
2. Go to Settings > Pages
3. Set source to "Deploy from a branch" > main > / (root)
4. Access at `https://[username].github.io/jp-carousel-studio/`

## No Dependencies

Everything runs from a single `index.html` file. Libraries loaded from CDN:
- html2canvas 1.4.1
- Google Fonts

No build step required.
