# Anti-Slop for Mockup Slicing

Slop is anything generic or off-mockup. The mockup is the only source of truth, so whatever does not come from it is slop.

## Visual

- No gradients, glassmorphism, glows, blur, or shadows absent from the mockup.
- No default palettes (purple-blue, indigo, Bootstrap blue, Tailwind grays) unless they are the mockup's colors; every color is an audit hex.
- No Inter/Roboto/Arial as a "safe" font when the mockup uses another.
- Do not normalize radius, spacing, or sizes; use the measured numbers, including untidy ones. Do not "tidy up" deliberate misalignment (overlap, offset, rotation).
- No animations, transitions, parallax, hover effects, or scroll reveals absent from the mockup. If the mockup shows hover/active/focus states, copy them exactly; otherwise none, except `cursor: pointer` on clickable elements.
- No emoji, no decorative icons. Icon rules: `output-structure.md`.

## Content

- Copy text exactly (capitalization, punctuation, numbers); do not rewrite, correct, or translate it.
- No sections, buttons, badges, social links, newsletters, or pages that are not in the mockup.
- **Interactive components (tab, accordion, pagination, filter).** One test: does the mockup contain the content the behavior needs?
  - **Yes: build the behavior.** Tabs when every panel's content is shown; accordion when its items and answers are shown; a filter when its categories come from the visible content. The behavior is part of the component, not an addition.
  - **No: build it static, exactly as shown** (for example only one tab/accordion state is visible, or pagination points to pages the mockup lacks). Never invent text, panels, or pages; report it and offer to wire it up once the content is provided.
  - Minimal behavior: short vanilla JS, no library, no animation unless the mockup shows one, and never rewrite or generate visible text (such as "1-N of N results").
  - Accessible and keyboard operable: `tablist`/`tab`/`tabpanel` with `aria-selected` for tabs, `<button aria-expanded>` for accordions.
- No lorem ipsum or invented data. If text is illegible, flag it and ask or report.
- No stock photos or external image URLs (Unsplash, placeholder.com); missing assets follow `output-structure.md`.
- Links with no destination use `href="#"`.

## Code

- Follow `code-standards.md`. No stacks of meaningless `div`s.
- No unneeded libraries (jQuery, AOS, Swiper, full Font Awesome). No JS for what HTML/CSS can do.
- No excessive comments, dead code, or unused classes; comments only as section markers.
- No inline `style` attributes (for example `<col style="width: 144px">`); put every value in CSS (a class, or a Tailwind utility). The only exception is a value that must be set per element by JS at runtime.
- No `!important` unless needed to override a framework.
- Do not change the number or order of elements for coding convenience; the HTML follows the mockup's visual structure.

## Final check (before delivering)

For every element and style ask: "Where in the mockup is this?" No answer means remove it.

- [ ] Typography, colors, spacing, image sizes, radius, and shadows match the audit; text identical to the mockup
- [ ] Every icon is SVG and under the size budget (or reported)
- [ ] Headings in order, every `img` has `alt`, every `input` has a `label`; BEM consistent (except Tailwind)
- [ ] No inline `style`, no tag/ID selectors for components
- [ ] Interactive components pass the content test, with no invented text
- [ ] No horizontal scroll from 320 px to the mockup width
- [ ] Nothing beyond the mockup

Allowed exception: invisible things needed for the code to work on other devices (viewport meta, accessibility attributes, a mobile menu state when no mobile frame exists). Report every exception with visual impact.
