# JP Carousel Studio — Claude Code Build Brief
**Version:** 4.0  
**Target:** D:\OneDrive - Abnaasi\Abnaasi Pty Ltd\Ai Projects\jp-personal-posts\JP-Carousel-Studio  
**References:** D:\OneDrive - Abnaasi\Abnaasi Pty Ltd\Ai Projects\jp-personal-posts\references  
**Owner:** Jagminder Singh (JP) — CTO & Founder, Abnaasi Pty Ltd  

---

## CRITICAL: Read existing work first

**Before writing any code**, read the existing file in this directory:
```
jp-carousel-studio-v3.html
```

This file is the current working version (v3) built in Claude.ai. It already contains:
- 14 themes (dark-editorial, warm-cream, deep-navy, chalk, pitch-black, luxury-gold, saffron, forest, linen-tutorial, linen-minimal, magazine, breaking, tech-dark, burgundy)
- 8 background styles (plain, grid, dots, shapes3d, circles, gradient, glow, multi-glow, diagonal, noise)
- 8 headline font options
- Multi-colour gradient builder with 3 colour pickers, opacity slider, direction selector, 8 preset glow swatches
- html2canvas PNG export (single slide + all slides with progress bar)
- Named draft system saved to localStorage
- Export Draft as JSON + Import Draft from JSON
- Dark/light mode toggle
- Keyboard shortcuts
- 8 slide types: hook, stats, bullets, cta, quote, grid-cards, problem, solution
- Modular slide content editing in left panel
- Right panel with post brief, LinkedIn caption generator, export guide

**Your job is to build v4.0 as a new file `index.html`**, using the v3 file as the foundation. Preserve everything that already works. Add what is missing (listed below). Do not rebuild from scratch what already works.

**Specifically:**
- Copy the theme system, gradient builder, export logic, draft system, dark/light mode, and keyboard shortcuts directly from v3
- Extend the slide type system with the missing block types listed below
- Fix any known issues from v3 (see Known Issues section)
- Add the new features listed below

---

## What to build

A **local-first carousel post studio** that runs in Chrome as a single HTML file (no server, no Node required to run). It should also be deployable to GitHub Pages as a static site. The output is 1200×1200px PNG slides ready for LinkedIn document post upload.

Study every image in the references folder before writing any code. The visual style of the output slides must match the reference designs closely.

---

## Known issues in v3 to fix in v4

1. **Slide fields are fixed templates** — v3 renders fields based on a single `type` per slide. Replace with the modular block system described below so users can combine any blocks freely on each slide.
2. **Image slide type** — v3 has the type defined but no real image upload. Implement properly with file picker → base64 → displays in canvas → exports correctly.
3. **Progress bar** — works but only visible on 5+ slides (fast for small sets). Add a minimum visible delay so it always shows briefly.
4. **Linen-tutorial theme** — numbered step rows partially implemented. Ensure they render as individual cards with left-aligned number in accent colour.
5. **Tech-dark theme** — gradient headline (CSS `-webkit-text-fill-color`) does not capture in html2canvas. Detect this and fall back to solid accent colour for export.
6. **Draft list** — only shows 6 drafts in left panel. Show all with scroll.

---

## New features to add in v4 (not in v3)

### 1. Modular block system (most important)
Replace the fixed slide type system with a block-based compositor. Each slide contains an ordered list of blocks. Users can add, remove, and reorder blocks within each slide. See the full block library below.

### 2. Missing block types (add these)
- `numbered-steps` — 3–4 numbered cards, number in accent colour left-aligned
- `step-pill-tag` — centred rounded pill with border (e.g. "STEP 1 -- INSTALL")
- `code-block` — macOS window with red/amber/green dots, title bar, monospace content area
- `comparison-cols` — two-column layout: left (green dots, "NOT leaked" style) vs right (orange dots)
- `tag-pills-footer` — row of rounded outline pills at bottom (e.g. #MetaAI, #TRIBEv2)
- `image-block` — drop zone with file picker, base64 encoded, renders in canvas
- `image-split` — left 45% text, right 55% uploaded image
- `highlight-bar` — full-width accent background bar with white text
- `body-text` — plain sans-serif paragraph (not monospace) for aiwithanushka/Anthropic style
- `diagram-flow` — simple SVG: 3–4 labelled boxes connected by arrows in a row or circle

### 3. Per-slide theme override
Each slide can inherit the global theme (default) or override with its own theme. Add a small toggle per slide: "Use global theme / Override" with a mini theme picker.

### 4. Drag to reorder slides
HTML5 drag and drop on slide tabs. Dragging a tab reorders slides. Visual drop indicator.

### 5. Slide duplicate button
Add a "Duplicate" button per slide tab. Copies the slide and all its blocks.

---

- **Single file:** `index.html` — all HTML, CSS, JS in one file. No build step required to run.
- **Libraries loaded from CDN:**
  - `html2canvas` 1.4.1 — for PNG export
  - Google Fonts — Playfair Display, DM Sans, JetBrains Mono, Bebas Neue, Syne, Fraunces, DM Serif Display, Outfit, Inter, Space Grotesk
- **Optional:** If you determine a React/Vite build would be significantly better, create it as a proper project with `npm run dev` and `npm run build`, outputting a `dist/index.html` that is fully self-contained.
- **Storage:** Browser localStorage for drafts. No backend, no API keys.

---

## Core architecture — Modular Block System

**This is the most important architectural decision.** Do NOT use fixed slide templates. Instead implement a modular block system:

Each slide has:
1. A **slide header** (tag + counter) — always present
2. A **headline block** — always present  
3. **1 to 4 content blocks** chosen freely from a block library
4. A **slide footer** (handle + swipe indicator) — always present

Users add/remove/reorder content blocks within each slide. This matches how the reference designs actually work — each slide is independently composed, not templated.

### Content Block Library (implement all of these):

| Block type | Description | Reference |
|---|---|---|
| `sub-text` | Monospace sub-caption with left orange border | All Ajay/thevibefounder refs |
| `body-text` | Regular sans-serif paragraph, larger line-height | aiwithanushka, Anthropic refs |
| `stat-row` | 3 stat cards (value + label). Values in accent colour | Refs 1-8, 26-34 |
| `bullet-box` | Dark card with box label + 3 bullet points | Refs 1-8 |
| `numbered-steps` | 3-4 numbered rows, each in own card | aiwithanushka Refs 9-14, 18-21 |
| `step-pill-tag` | Centred pill label (e.g. "STEP 1 -- INSTALL") | aiwithanushka Refs 9-14 |
| `code-block` | macOS window (red/amber/green dots + title bar) + monospace content | aiwithanushka Refs 10, 11 |
| `comparison-cols` | Two columns: left (green dots) vs right (orange dots) | Refs 32-33 |
| `grid-cards` | 2×2 card grid, one card can have accent border | Refs 28, 30 |
| `cta-box` | Centred box with "comment this word" + large keyword + sub-hint | All refs |
| `quote-block` | Large italic quote + attribution | - |
| `tag-pills-footer` | Row of rounded pill tags (e.g. #MetaAI, #TRIBEv2) at bottom | Refs 29, 34 |
| `image-block` | Image area with file picker (base64 encode, stores in state) | Refs 26, 22-25 |
| `image-split` | Left 45% text headline, right 55% uploaded image with overlay | Refs 26, 27 |
| `inline-code` | Inline monospace code snippet with dark bg + accent border | Refs 30-33 |
| `highlight-bar` | Full-width accent-coloured bar with white text inside | aiwithanushka Ref 21 |
| `diagram-flow` | Simple SVG flow diagram: boxes connected by arrows (user defines labels) | Refs 25-26 |

---

## Themes (implement all 14)

Read the reference images carefully. Each theme must feel authentic to the source material, not just a colour swap.

| Key | Name | Source creator | Background | Accent | Headline font |
|---|---|---|---|---|---|
| `dark-editorial` | Dark Editorial | @thevibefounder | #141414 | #c76a25 | Playfair Display |
| `warm-cream` | Warm Cream | @thevibefounder | #f5ede0 | #c76a25 | Playfair Display |
| `deep-navy` | Deep Navy | Original | #0d1b2a | #facb67 | Playfair Display |
| `chalk` | Chalk White | Original | #f7f4ef | #c76a25 | Playfair Display |
| `pitch-black` | Pitch Black | Original | #000 | #c76a25 | Bebas Neue |
| `luxury-gold` | Luxury Gold | Original | #0e0b00 | #e0b842 | Fraunces |
| `saffron` | Saffron Bold | Original | #e87d00 | #fff | Syne |
| `forest` | Forest Dark | Original | #0a1a0e | #4aaa4a | DM Serif Display |
| `linen-tutorial` | Linen Tutorial | @aiwithanushka | #eee8df | #c4622a | Outfit Bold |
| `linen-minimal` | Linen Minimal | Anthropic official | #e8e0d0 | #b85c38 | DM Serif Display |
| `magazine` | Magazine Cover | @Parasmadan.In | #1a1a18 | #e8622a | Playfair Display |
| `breaking` | Breaking News | Dark news style | #0d0d0b | #e8622a | Outfit Bold |
| `tech-dark` | Tech Dark | @speedy_devv | #1c1c1e | #ff7a35 | Syne (gradient headline) |
| `burgundy` | Burgundy | Original | #1a0a0e | #e8b4a0 | Playfair Display |

Theme-specific details:
- `linen-tutorial`: Step pill tag centred at top. Numbered steps in individual cards. Tag pills row at bottom.
- `breaking`: Breaking news pill with red dot at top (like BBC/CNN breaking). Large CTA slide has single orange glow dot + giant orange word centred.
- `tech-dark`: Headline uses CSS gradient text (orange to coral). Dot grid background. Avatar circle at bottom-left.
- `magazine`: Faint grid overlay on background. Italic body text (not monospace). Split layout supported.

---

## Background options

Implement all of these as SVG overlays:

| Key | Description |
|---|---|
| `plain` | No overlay |
| `grid` | Faint grid lines in accent colour |
| `dots` | Dot grid in accent colour |
| `diagonal` | Diagonal lines |
| `circles` | Concentric ghost circles top-right + bottom-left |
| `shapes3d` | Ghost 3D wireframe shapes: cube top-right, sphere bottom-left, torus mid-right, triangle bottom-right, dot cluster top-left |
| `gradient` | 2-point radial gradient using custom colours |
| `glow` | Single central glow using custom colour |
| `multi-glow` | 3 overlapping radial glows at different positions using 3 custom colours |
| `noise` | SVG feTurbulence noise overlay |

---

## Gradient / Glow Builder

Dedicated panel section with:
- 3 colour pickers (c1, c2, c3)
- Opacity slider (5–60%)
- Direction selector (Radial / 135deg diagonal / to bottom / to right)
- 8 preset glow swatches as small coloured circles:
  1. Amber Fire: #c76a25 + #facb67 + #7a1c1c
  2. Electric Blue: #4a80ff + #8a40ff + #1a0060
  3. Aqua Teal: #00c896 + #0080ff + #001a30
  4. Rose Purple: #ff4080 + #8040ff + #1a001a
  5. Gold Luxury: #e0b842 + #c06020 + #0e0b00
  6. Neon Green: #40ff80 + #20c040 + #001a10
  7. Dual Diagonal: #ff4040 + #4040ff + #0a0a1a
  8. White Halo: #ffffff + #aaaaaa + #000
- Clicking a preset auto-applies it and switches background to `multi-glow`

---

## Fonts

Headline font selector — 8 options with live preview in correct font:
Playfair Display, Fraunces, Bebas Neue, Syne, Outfit, DM Serif Display, Inter, Space Grotesk

Body/caption font is always DM Sans. Tag/counter font is always JetBrains Mono.

Font size is adaptive per font — Bebas at ~52px, others at ~34px for headline.

---

## Image handling

For `image-block` and `image-split` block types:
- Show a styled dashed drop zone: "Drop image here or click to upload"
- On click: file picker (accepts image/*)
- On file select: FileReader → base64 → store in slide block state
- Render as `<img>` or CSS `background-image` in the slide
- base64 image persists in draft JSON export/import
- html2canvas captures it correctly because it's inline base64

---

## Slide management

- Add slide (up to 12)
- Remove last slide
- Drag to reorder slides (use HTML5 drag and drop on slide tabs)
- Each slide independently has its own theme override option (inherits global theme by default, can override per-slide)
- Slide types in tabs displayed as: `01 Hook`, `02 Stats`, etc. (based on first block type)

---

## Export

### PNG export (primary)
- Uses html2canvas at scale 2.6 (outputs ~1200×1200px)
- "Export This Slide" — downloads current slide as PNG
- "Export All Slides" — downloads all slides with progress overlay showing percentage
- Filenames: `jp-slide-01-hook.png`, `jp-slide-02-stats.png`, etc.

### Draft save/load
- Save to localStorage (named, up to 25 drafts, never overwrites)
- Export Draft as JSON — downloads `.json` file to PC (includes all state + gradient state + any base64 images)
- Import Draft from JSON — file picker, loads everything back
- Draft list shows name + date, with load and delete buttons

---

## Studio UI layout

```
[TOP BAR: brand name | Drafts | Save | Export Draft | Import Draft | Export Slide | Export All | 🌙]

[LEFT PANEL 280px] | [CENTRE flex] | [RIGHT PANEL 250px]
```

### Left panel (scrollable):
1. Profile (handle, tag)
2. Global Theme (14 swatches + Shuffle)
3. Background (pills)
4. Gradient/Glow Builder (always visible, not hidden)
5. Headline Font (8 font pills, rendered in actual font)
6. Slide Manager (tabs, add/remove, drag hint)
7. Current Slide Editor:
   - Slide type tabs to navigate
   - Per-slide theme override toggle
   - Block list for current slide: each block shows its type + a remove button
   - "Add Block" dropdown listing all block types
   - Fields for each block (dynamically rendered)
8. Drafts (list + name input + save button)

### Centre (canvas area):
- 460×460px canvas frame
- Slide navigation dots
- ← → arrows
- "Slide X of Y" label
- Export row (Export This Slide + Export All)

### Right panel (scrollable):
- Post brief (list of all slides with first headline)
- LinkedIn caption auto-generator (assembled from slide content)
- Research workflow tip (link to claude.ai)
- Keyboard shortcuts reference
- Export guide

---

## Dark/Light mode

Toggle button (🌙/☀️) in top bar. Keyboard shortcut D.
Applies to the studio UI only — slide canvas always uses the selected theme.
CSS variables approach.

---

## Keyboard shortcuts

- ← → navigate slides
- 1-9 jump to slide
- S save draft
- D toggle dark/light
- E export current slide

---

## GitHub setup

After building the studio, do the following:
1. Initialise a git repo in the project folder
2. Create `.gitignore` (ignore node_modules if present, ignore .env)
3. Create `README.md` with brief description and how to use
4. Make initial commit with message: "feat: JP Carousel Studio v4.0 — initial build"
5. Output instructions for JP to:
   a. Create a GitHub repo at github.com/new named `jp-carousel-studio`
   b. Push to remote: the exact commands to run
   c. Enable GitHub Pages from the main branch root (Settings → Pages)
   d. Access URL will be: `https://[username].github.io/jp-carousel-studio/`

---

## Quality standards

- No generic AI slop aesthetics. The output slides must look like the reference images.
- Test every theme visually before finishing — if a theme looks generic, fix it.
- All 14 content block types must render correctly in the canvas.
- Export must produce clean PNGs — test html2canvas capture on at least 3 slide types.
- The studio UI must be stable at 1280×800px browser window (minimum usable size).
- Fonts must load correctly (use Google Fonts CDN with display=swap).
- No JavaScript syntax errors. Run a self-check before declaring done.
- If building as React/Vite, `npm run build` must produce a working `dist/index.html`.

---

## What NOT to do

- Do NOT use React if a clean vanilla JS/HTML solution achieves the same result — simpler is better for a local tool
- Do NOT add authentication, user accounts, or any cloud sync
- Do NOT use Tailwind CSS (no build step for vanilla version)
- Do NOT hardcode content — all text is user-editable
- Do NOT ignore the reference images — they are the design brief
- Do NOT add AI content generation inside the studio — content comes from Claude.ai separately

---

## Session end

When done, confirm:
1. `index.html` exists and opens correctly in Chrome
2. v3 features all preserved (themes, gradients, export, drafts)
3. New block types all render correctly in canvas
4. PNG export works with html2canvas (test at least 3 block types including image-block)
5. Draft save/load works including JSON export/import
6. Git repo initialised with README
7. GitHub Pages deployment instructions printed clearly

---

## First command to run

```
Read jp-carousel-studio-v3.html carefully to understand all existing features, then read all images in the references folder, then build index.html as v4.0 following this CLAUDE.md.
```
