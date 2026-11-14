# Vanilla CSS

- Put audit tokens (colors, font families) in CSS variables on `:root`. Specific sizes are written directly where used, so they are easy to match against the mockup.
- Minimal reset only: `box-sizing: border-box`, body margin 0, `img { max-width: 100%; display: block; }`. No large reset that changes elements the mockup does not cover.
- Flexbox/Grid for layout. Desktop-first media queries if the mockup is desktop; mobile-first if the mockup is primarily mobile.
- Class naming: BEM (`code-standards.md`).
