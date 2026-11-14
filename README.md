# slice-mockup-html

A Claude skill that slices a mockup into **pixel-perfect, responsive HTML** that stays faithful to the design: no creative additions, no generic "AI look".

Give it a **PNG, an SVG, or a Figma file (via MCP)**. It asks how you want the output, audits the design before writing any code, then builds semantic, BEM-structured HTML/CSS for desktop, tablet, and mobile.

> Status: v1.0.0. . Feedback and examples are welcome.

## Why

Generated HTML often looks "close" but drifts from the design: guessed fonts, rounded spacing, invented sections, default purple gradients. This skill treats the mockup as the only source of truth and checks the result against it.

## Features

* **Two upfront questions**: single file (inline CSS/JS plus `assets/`) or separate `html`/`css`/`js` files; vanilla CSS, Bootstrap (CDN), or Tailwind (CDN).
* **Audit before coding**: fonts, colors, sizes, weights, spacing, radius, shadows, assets. Every value is marked *measured* or *estimated*.
* **Three input types**: Figma via MCP (exact values), SVG (parsed from the XML), PNG (measured with pixel analysis).
* **Pixel perfect at the mockup width**, responsive for tablet and mobile with minimal derived changes.
* **Semantic HTML and BEM** (BEM is not used with Tailwind).
* **Icons are always SVG**, cleaned and size-checked.
* **Anti-slop rules**: nothing that is not in the mockup, text copied exactly, no stock photos, no inline styles, no unnecessary libraries.
* **Interactive components** (tabs, accordion, pagination, filter) get working behavior only when the mockup contains the content they need; otherwise they stay static and are reported.
* **Verification**: renders the result and compares it with the mockup when the environment allows it.
* **Short final report**: unidentified fonts, estimated values, placeholders, and anything derived.

## Install

The skill is the folder [`slice-mockup-html/`](slice-mockup-html). Its folder name matches the `name` in `SKILL.md`, which Claude requires.

### claude.ai and Claude Desktop

1. Download `slice-mockup-html.skill` (or zip the `slice-mockup-html/` folder so the folder itself is inside the ZIP, not the loose files).
2. Make sure code execution is enabled in **Settings → Capabilities** (skills do not appear without it).
3. Upload the ZIP in the Skills section of Settings and toggle it on. Menu labels may change; see the [official guide](https://claude.com/docs/skills/how-to.md).

### Claude Code

Copy the folder:

```bash
# personal (all projects)
cp -r slice-mockup-html \~/.claude/skills/

# or per project
cp -r slice-mockup-html .claude/skills/
```

Then ask Claude Code which skills are available to confirm it is recognized.

## Usage

Attach your mockup and say what you want, in any language. Examples:

```
Slice this mockup into HTML.
```


Claude will ask for the output format and the CSS approach, audit the design, build the code, verify it, and report what it assumed.

For best accuracy use Figma via MCP or an SVG. With a PNG, values are measured and a few will be estimates (and icons are recreated as SVG).

## Results

In one test on a 1440 px desktop mockup (an SVG), the rendered result matched the mockup like this:

|Check|Result|
|-|-|
|Page height|3257 px, identical to the mockup|
|Pixels differing noticeably|about 1% (mostly anti-aliasing on text edges)|
|Horizontal scroll at 1440 / 768 / 375 px|none|

Measured by rendering in Chromium and comparing pixels. One test is not a benchmark; please add your own in [`examples/`](examples).

## How it works

1. Receive the mockup (PNG, SVG, or Figma via MCP).
2. Ask the two output questions.
3. Audit the mockup (values marked measured or estimated).
4. Write the code following the fidelity, code-standard, and anti-slop rules.
5. Verify by rendering and comparing with the mockup.
6. Deliver the files plus a short report.

The skill keeps its footprint small: `SKILL.md` holds the workflow and points to reference files that are loaded only when needed (only the chosen CSS framework is read). A typical run loads roughly 4,800 tokens of instructions (estimate).

## Repository layout

```
slice-mockup-html/
├── SKILL.md                      # workflow
└── references/
    ├── mockup-audit.md           # reading PNG / SVG / Figma, measuring, fonts
    ├── output-structure.md       # file structure, assets, SVG icons
    ├── responsive-and-fidelity.md
    ├── code-standards.md         # semantic HTML, BEM
    ├── anti-slop.md              # prohibitions and final check
    ├── vanilla.md  bootstrap.md  tailwind.md
examples/                         # add your own mockups and results
```

## Limitations

* Mockups from PNG rely on measurement; fonts that cannot be identified are replaced by the closest match and reported.
* Icons from PNG are recreated by hand as SVG; very complex icons may not be reproducible faithfully.
* Rendering and pixel comparison need a browser in the environment; if unavailable, the skill checks through code and says so.
* Tablet and mobile layouts are derived when the mockup only provides one size.
* Tested so far on a limited number of mockups.

## Contributing

Issues and pull requests are welcome, especially:

* new examples with mockup, result, and notes,
* cases where the output drifted from the mockup (include the mockup if you can share it),
* improvements that keep the skill small.

## License

[MIT](LICENSE) © \Dwiky Dias Laksmana

