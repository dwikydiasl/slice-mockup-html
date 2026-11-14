# Responsive, Pixel Perfect, and Verification

## 1. Pixel perfect at the mockup width

- At the mockup's canvas width (for example 1440 px), every size equals the audit: font-size, line-height, letter-spacing, padding, margin, gap, radius, button height, image size.
- Use `px` for these. Avoid `rem`/`em`/`%` that produce fractions drifting from the mockup unless truly equivalent.
- Container: fixed `max-width` from the mockup, centered, with side padding. Section backgrounds may stretch full-bleed if the mockup does.
- Commonly missed: per-line `line-height`, heights set by padding (not fixed), icon sizes, border thickness, vertical text position inside buttons.
- `font-display: swap` plus a fallback stack to avoid layout jumps.

## 2. Tablet and mobile

If the mockup has tablet/mobile frames, follow them exactly and use their widths as breakpoints.

If it has one size, derive the rest with minimal changes and no added or removed content:
- Side-by-side columns stack (2 on tablet, 1 on mobile when content does not fit).
- Horizontal nav becomes a hamburger only if it does not fit; keep the open menu minimal and note it in the report.
- Headings scale down proportionally (for example 56, 40, 32); body not below 14 px; section padding reduced proportionally; mobile side padding 16 to 24 px.
- Images and cards follow column width, keeping ratio. Wide tables scroll inside their own wrapper, never on the body.

Reference breakpoints: tablet 768 to 1023 px, mobile below 768 px; also check 375 and 320 px.

Safe rules:
- `<meta name="viewport" content="width=device-width, initial-scale=1">`.
- No horizontal scroll from 320 px up to the mockup width.
- No fixed width/height on text-containing elements; use `max-width`, `min-height`, or let content decide.
- Images: `max-width: 100%; height: auto`, `object-fit` per mockup.
- `overflow-wrap: anywhere` where long text is risky (emails, URLs).
- Touch targets about 40 to 44 px on mobile, as long as the look matches the mockup.

## 3. Verification

1. **Render**: try Playwright (`pip install playwright --break-system-packages`, `playwright install chromium`). If the browser download or CDNs (Bootstrap/Tailwind/Google Fonts) are blocked by the sandbox, do not force it: check through the code and state that rendering was not tested.
2. **Screenshots** at the mockup width, 768, and 375.
3. **Compare** with the mockup per section, or with PIL (`ImageChops.difference`) to find deviating areas.
4. Fix only the deviating parts; do not rewrite whole files.
