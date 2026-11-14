# Mockup Audit (before coding)

Replace guesses with data. Finish this before writing any HTML.

## 1. Read the input (most accurate first)

Priority: Figma via MCP, then SVG, then PNG. Never drop to a lower level if a higher one is available.

**Figma via MCP**
- Figma tools are deferred: load them with `tool_search` (for example `figma design context`) first.
- `get_design_context` for structure, fonts, fills, strokes, radius, effects, and auto-layout padding/gap; `get_variable_defs` for tokens; `get_screenshot` for a visual reference; `download_assets` for icons and images.
- These are **measured** values. Map Figma variables/styles to CSS variables or `tailwind.config` names instead of scattering raw hex.
- Note which frames are desktop, tablet, or mobile.
- If Figma cannot be read, say so and ask the user for PNG or SVG.

**SVG file**
- Parse the XML (Python `xml.etree.ElementTree`): `viewBox`, `width`, `height` give the canvas size (check for a scale factor). Read `x`, `y`, `width`, `height`, `rx`, `fill`, `stroke`, `font-family`, `font-size`, `font-weight`, `letter-spacing` directly, and account for parent `transform`s.
- Distances between elements come from coordinates. Values read this way are **measured**.
- If text is converted to outlines, the font cannot be read: identify it visually (section 3) and mark it as an estimate.
- Render to PNG (`cairosvg` or a headless browser) and `view` it to confirm the look.

**PNG file** (values are measured by hand; see section 2)

## 2. Measuring a PNG

Look with `view`, then measure with PIL via bash (the file is in `/mnt/user-data/uploads`). Do not eyeball.

- **Canvas**: `Image.open(path).size`. Watch the export scale: 2880 px wide usually means @2x of 1440, so halve every measurement.
- **Colors**: sample a pixel in a flat area (not on anti-aliased text edges); for text, the darkest/lightest pixel in the middle of a thick stroke. Convert to hex.
- **Distances and sizes**: scan pixel rows/columns for color changes, or crop and enlarge the area to read its boundary. Split tall mockups into per-section crops.
- **Font size**: cap-height is roughly 0.70 to 0.72 of font-size for most sans-serifs. Line-height is the baseline-to-baseline distance of consecutive lines.
- **Patterns**: values usually follow a scale (multiples of 4 or 8). If a measurement is 19 among multiples of 8, it is probably 20.

## 3. Identifying fonts

- Read the name from Figma or source files when available.
- Otherwise use clues (shape of "a", "g", "R", "1", the tail of "Q"). Commonly confused: Inter / SF Pro / Roboto / Helvetica; Poppins / Montserrat; DM Sans / Plus Jakarta Sans.
- Load from Google Fonts with only the weights actually used, plus a proper fallback stack.
- If unsure or the font is paid/custom, use the closest match and **report it as an estimate**.

## 4. Audit spec template

```
CANVAS      : 1440 px, container 1200 px, side margin 120 px
GRID        : 12 columns, gutter 24 px
COLORS      : primary #1E3A5F (measured) | muted #6B7280 (estimated)
FONTS       : Heading "Plus Jakarta Sans" 700 | Body "Inter" 400/500
TYPOGRAPHY  : h1 56/64 -1% | body 16/24   (font-size/line-height)
SPACING     : section 96 px | card gap 24 px | card padding 32 px
RADIUS/SHADOW: button 8 px | card 16 px | 0 4px 24px rgba(0,0,0,.08) (estimated)
ASSETS      : hero.jpg (crop), 6 icons (SVG)
INTERACTION : navbar hamburger, FAQ accordion
```

## 5. Common mistakes

- Treating image size as design size (missing @2x).
- Using a "similar" framework color instead of the measured hex.
- Missing letter-spacing on headings, 1 px borders, dividers, badges, arrows, active nav states, the footer, and sticky/floating elements.
- Treating cards/sections as identical when their spacing differs slightly.
