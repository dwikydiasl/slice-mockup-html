# Changelog

All notable changes to this project are documented here.
Format based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), versioned with [Semantic Versioning](https://semver.org/).

## [1.0.0] - 2026-11-14

### Added
- Seven-step workflow: receive mockup, ask output questions, audit, write code, verify, deliver with a short report.
- Two upfront questions: output format (single file or separate files) and CSS approach (vanilla CSS, Bootstrap CDN, Tailwind CDN).
- Mockup inputs: PNG, SVG, and Figma via MCP, each with its own way of reading exact values.
- Audit step (fonts, colors, sizes, spacing, assets) before any code is written.
- Pixel-perfect and responsive rules (desktop, tablet, mobile), with verification by rendering and pixel comparison.
- Code standards: semantic HTML everywhere, BEM for vanilla CSS and custom Bootstrap classes (not used with Tailwind).
- Icons are always delivered as SVG, with a viewBox normalization rule and a size budget.
- Anti-slop rules: no additions beyond the mockup, no generic palettes or fonts, no inline styles.
- Content test for interactive components (tab, accordion, pagination, filter): build the behavior only when the mockup contains the content it needs, otherwise keep it static and report it.

### Changed
- References are split per framework so only the chosen one is loaded, reducing a typical run from about 7,300 to about 4,800 tokens (estimate).
