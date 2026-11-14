# Output Structure, Assets, and Icons

## 1. File structure

```
Single file                     Separate files
project/                        project/
├── index.html (<style> in      ├── index.html
│   <head>, <script> before     ├── css/style.css
│   </body>)                    ├── js/main.js
└── assets/                     └── assets/
    ├── images/                     ├── images/
    ├── icons/                      ├── icons/
    └── fonts/ (local fonts only)   └── fonts/
```

- Asset paths are relative (`assets/images/hero.jpg`), no leading slash.
- No interaction means no `main.js` and no `<script>`.
- One page = one `index.html`; extra pages get descriptive names (`about.html`).
- CSS order follows the page: tokens, minimal reset, global components, sections top to bottom, media queries last.

## 2. Assets

Use user-supplied assets as-is (icons still end up as SVG, see section 3). When slicing from the mockup itself:

- **Figma**: `download_assets` (SVG for icons and simple vector logos; PNG/JPG at the highest scale for photos and complex illustrations).
- **SVG mockup**: cut vector elements into standalone SVG files; decode embedded raster `<image>` data into `assets/images/`.
- **PNG mockup**: crop photos, illustrations, and logos with PIL at native resolution (PNG for transparency/illustration, JPG for photos).
- Never crop text, buttons, or cards as images; build them in HTML/CSS.
- A photo covered by an overlay or text cannot be cropped cleanly: use a neutral gray placeholder of the same ratio (never a stock photo) and report it.
- Descriptive lowercase names (`hero-bg.jpg`), `width`/`height` or `aspect-ratio` to avoid layout shift, meaningful `alt` (`alt=""` if decorative). Decorative backgrounds via CSS `background-image`, content images via `<img>`.

## 3. Icons: always SVG

Never PNG/JPG, an icon font, or an emoji.

**Getting the SVG**
- **Figma**: export the node with `download_assets` as SVG (exact).
- **SVG mockup**: extract the icon's `<g>`/`<path>` into its own file, keep the path data untouched, set `viewBox` to its bounding box.
- **PNG mockup**: recreate by hand from the measured geometry (paths, circles, rounded rects, strokes); zoom in to read stroke width, corners, and proportions. For simple monochrome icons `potrace` may be used if available, but clean the output (few nodes). If an icon is too complex to recreate faithfully, say so in the report; never silently deliver a raster icon.

**SVG rules**
- Normalize `viewBox` to `0 0 w h` (Figma and SVG exports keep absolute canvas coordinates such as `314 2049 20 20`; shift the coordinates or wrap the content in a `translate` group). `width`/`height` equal the size in the mockup (for example 24 px).
- Remove editor junk (metadata, `sodipodi`/`inkscape` attributes, empty groups, unused `defs`, clashing `id`s).
- Icons that follow text color or change on hover/active use `currentColor`; fixed brand colors keep the audited hex.
- Keep stroke icons as strokes (`stroke-width`, `linecap`, `linejoin` as drawn).
- **Size budget**: an icon above about 10 KB must be optimized (`npx svgo`, or simplify paths and drop invisible blur/filter effects) without changing the look at its displayed size. If it is still above the budget, keep it and name the file and its size in the report. Run `svgo` on every icon when available.

**Placement**
- Inherits color or has hover/active states: inline `<svg>`.
- Static with fixed colors: file in `assets/icons/`, used as `<img src="assets/icons/icon-search.svg" alt="" width="24" height="24">`.
- Repeated many times: inline `<symbol>` + `<use href="#icon-name">` is allowed. No CSS sprites or icon fonts.
- Single-file output prefers inline SVG.
- Accessibility attributes: see `code-standards.md`.
