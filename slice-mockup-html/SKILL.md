---
name: slice-mockup-html
description: Slice a mockup/design (PNG file, SVG file, or Figma via MCP) into pixel-perfect, responsive HTML (desktop, tablet, mobile) that is 100% faithful to the mockup, with no creative additions and no generic AI look (anti-slop). The skill first asks for the output format (single file or separate files) and the CSS approach (vanilla CSS, Bootstrap CDN, or Tailwind CDN), then audits the mockup (fonts, colors, sizes, spacing) before writing any code. Use this whenever the user mentions slice, slicing, slicing mockup, convert design to HTML, mockup to HTML, design to code, Figma to HTML, "buatkan HTML dari desain ini", "sesuai mockup", or "pixel perfect", even if they do not say "skill" or "slice".
---

# Slice Mockup to HTML

Copy a mockup into HTML faithfully; do not redesign it. A correct result, overlaid on the mockup, shows almost no difference. Creativity, "improvements", and extra features count as mistakes.

## Workflow (in order, do not skip ahead)

### 1. Receive the mockup

Confirm the mockup actually exists (check `/mnt/user-data/uploads` and the conversation); if not, ask for it, never guess. If it has many pages/frames, ask which to build.

Supported inputs: **PNG, SVG, Figma via MCP** (JPG is treated as PNG). For any other format, ask the user to export PNG or SVG. How to read each input is in `references/mockup-audit.md`. For Figma, load the `figma:figma-design-to-code` skill before calling `get_design_context`.

### 2. Ask the two questions

Ask both together, before the audit and before coding. Skip a question if the user already answered it. Use `ask_user_input_v0` if available (one call, two questions); otherwise ask in short plain text.

1. **Output format**: Single file (HTML, CSS, JS inline in one `index.html`, plus `assets/`) or Separate files (`index.html`, `css/style.css`, `js/main.js`, plus `assets/`)
2. **CSS**: Vanilla CSS, Bootstrap via CDN, or Tailwind via CDN

After the answers, read `references/output-structure.md` and the one matching file: `references/vanilla.md`, `references/bootstrap.md`, or `references/tailwind.md`. Do not read the other two.

### 3. Audit before coding

Read `references/mockup-audit.md` and produce the audit spec (canvas, container, grid, colors, fonts, spacing, radius, shadows, assets, visible interactions), each value marked **measured** or **estimated**. Show the user a summary only if a decision is needed (unidentifiable font, missing asset); otherwise continue.

### 4. Write the code

- Rules: `references/responsive-and-fidelity.md` (pixel-perfect and responsive), `references/code-standards.md` (semantic HTML, BEM), `references/anti-slop.md` (prohibitions). Apply them while writing, not afterwards.
- Values at the mockup width must equal the audit exactly (px, not rounded to a framework scale).
- JS only for interactive components visible in the mockup, following the content test in `anti-slop.md` (build the behavior when the mockup has the needed content, otherwise keep it static and report); no extra libraries beyond what the chosen framework requires.

### 5. Verify

Follow the verification steps in `references/responsive-and-fidelity.md`, then run the final check in `references/anti-slop.md`. If the sandbox cannot render, say so plainly.

### 6. Deliver

Save to `/mnt/user-data/outputs/` and share with `present_files`. For more than one file, package a single `.zip` with the folder structure intact (so asset paths stay valid) and also present `index.html`.

Then give a report of at most about 6 lines, only what the user needs to know: unidentified fonts and their substitutes, estimated values, placeholder assets, icons that could not be recreated or exceed the size budget, interactive behavior built or left static for lack of content, and tablet/mobile parts derived because the mockup lacks them. No feature suggestions, improvement ideas, or self-praise.

## Core principles

- **Fidelity above all.** If the mockup looks weak, copy it as-is.
- **Do not add** anything absent from the mockup; **do not guess silently**: every assumption with visual impact goes in the report.
- **Copy text exactly**, including capitalization, punctuation, and numbers; flag illegible text.
