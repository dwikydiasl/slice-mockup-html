# Code Standards: Semantic HTML and BEM

Semantic HTML applies to every CSS option. BEM applies only to classes we write ourselves (section 2).

## 1. Semantic HTML

Choose elements by **meaning**; appearance belongs to CSS.

Skeleton: `<header>` (logo + `<nav>`), one `<main>`, `<section aria-labelledby>` for each themed part with a heading, `<footer>`.

| Need | Element |
|---|---|
| Menu | `<nav>` with `<ul><li><a>` |
| Action (submit, toggle, open menu) | `<button type="button">` |
| Go to a page/anchor | `<a href>` (even if styled as a button) |
| Headings | `<h1>` to `<h6>` in order, no skipped levels |
| Cards/features/menu items | `<ul>`/`<ol>` + `<li>` |
| Standalone card (article, product) | `<article>` |
| Image + caption | `<figure>` + `<figcaption>` |
| Image | `<img>` with `alt`, `width`, `height` |
| Form | `<form>`, `<label for>` per `<input>`, correct input types |
| Quote, date, contact | `<blockquote>`, `<time datetime>`, `<address>` |
| Tabular data | `<table>` with `<thead>`, `<th scope>` |

Rules:
- One `<h1>` per page. If the visual size does not match the level, fix it with CSS, not the level.
- Never wrap `<a>` in `<button>` or the reverse. No `div`/`span` with `onclick` instead of `<button>`.
- `<div>`/`<span>` only for layout or styling wrappers with no meaning; ask first whether a better element exists.
- Icons are SVG: decorative ones get `aria-hidden="true"`; icon-only buttons get `aria-label`.
- ARIA only when semantic elements are not enough.
- `lang="id"` or `lang="en"` on `<html>`, matching the mockup content.
- Accessibility attributes do not change appearance, so they do not violate "do not add beyond the mockup".

## 2. When BEM is used

| CSS | BEM |
|---|---|
| Vanilla | Mandatory for all classes |
| Bootstrap | Mandatory for custom classes; Bootstrap utilities (`row`, `col-md-6`, `d-flex`) stay as they are |
| Tailwind | Not used (styling is utilities). Custom classes only when unavoidable, such as JS hooks prefixed `js-` |

## 3. BEM rules

Format `block__element--modifier`, for example `.card`, `.card__title`, `.card--featured`.

1. Lowercase, hyphen-separated words: `.feature-list__item`.
2. **One element level only**: `.card__title`, never `.card__body__title`, even if nested in the HTML.
3. An element only lives inside its block.
4. A modifier is always used together with its base class: `class="button button--primary"`.
5. Name by function/content, not appearance: `.pricing__badge`, not `.red-box`.
6. No tag or nested selectors to style components (`.card h3`); give the element a class. No IDs for styling.
7. A repeated block (button, card) is written once and reused with modifiers. One block = one CSS section, in page order.
8. JS hooks use `js-` (`.js-menu-toggle`) and are never styled.

```html
<article class="plan plan--featured">
  <h3 class="plan__name">Pro</h3>
  <a class="button button--primary" href="#">Get started</a>
</article>
```
