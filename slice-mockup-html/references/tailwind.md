# Tailwind via CDN

```html
<script src="https://cdn.tailwindcss.com/3.4.17"></script>
<script>
  tailwind.config = { theme: { extend: {
    colors: { primary: '#1E3A5F', ink: '#1A1A1A' },
    fontFamily: { heading: ['"Plus Jakarta Sans"', 'sans-serif'], body: ['Inter', 'sans-serif'] }
    /* screens: only if the mockup breakpoints differ from the defaults */
  } } }
</script>
```
(Replace sample values with audit results.)

- Use **arbitrary values** to keep mockup numbers: `text-[56px] leading-[64px] tracking-[-0.01em] px-[120px] gap-[24px] rounded-[16px]`. Never round to the nearest scale (`text-5xl`, `p-8`) when the number differs.
- Register the mockup's colors and fonts in `tailwind.config` and use the names (`bg-primary`). No default palette (`blue-600`, `gray-500`) unless the hex matches.
- Preflight strips default styles, so declare headings, lists, and buttons explicitly.
- Breakpoints: sm 640, md 768, lg 1024, xl 1280, 2xl 1536. Put the mockup's desktop styles at the right breakpoint (`lg:` or `xl:`) and the unprefixed styles for mobile.
- No BEM here. Long class stacks are fine; no `@apply` or CSS file just for tidiness.
- `tailwind.config` always stays inline in `index.html` (the CDN reads it in the browser). Extra plain CSS (for example `@keyframes` the mockup needs) goes inline in single-file output, or in `css/style.css` for separate files.
