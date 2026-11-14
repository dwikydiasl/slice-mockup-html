# Bootstrap via CDN

```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
```
Load the JS bundle only if a component needs it (navbar collapse, accordion, modal).

Bootstrap defaults pull the result away from the mockup. Override:
- **Container**: default widths (540/720/960/1140/1320) rarely match; set `max-width` and padding to the mockup.
- **Typography**: `body` is 1rem/1.5 and headings have built-in margins and sizes; reset per the audit.
- **Colors**: no built-in colors (`btn-primary` blue, etc.) unless they match the mockup; override via Bootstrap CSS variables or your own classes.
- **Gutter**: set `--bs-gutter-x` to the mockup gutter.
- **Button, form, card, navbar**: override default padding, radius, and borders.
- Use utilities (`d-flex`, `row`, `col-md-6`) for structure and one custom stylesheet for all precise values. Do not stack spacing utilities whose rounding drifts from the mockup.
- Custom classes use BEM; Bootstrap utilities stay as they are (`code-standards.md`).
- Breakpoints: sm 576, md 768, lg 992, xl 1200, xxl 1400. Tablet = md, mobile = below md.
