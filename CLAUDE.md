# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page static marketing site for "M's by Marjorie's," a home bakery in Parañaque, Philippines. No build system, no package manager, no JavaScript framework, no backend — just `index.html`, `css/style.css`, and static images in `assets/`.

## Running / previewing

There is no build or dev-server tooling defined in the repo. To preview locally, serve the directory with any static file server, e.g.:

```
python3 -m http.server 8000
```

Then open `http://localhost:8000/index.html`. There is no lint, test, or build command — changes are visible directly by reloading the page.

## Architecture

- **`index.html`** — the entire site. One page with anchor-linked sections (`#top`, `#menu`, `#gallery`, `#order`, plus an unlinked `.showcase` section between the hero and menu) navigated via the sticky nav in-page; there is no routing or JS.
- **`css/style.css`** — all styling, organized by section (nav, hero, showcase, menu, gallery, order ticket, footer) with a shared design-token block at the top.
- **`assets/`** — logo and product photography referenced directly by `index.html`.

### Theming

Colors are defined as CSS custom properties on `:root` in `css/style.css` (cream/espresso/gold/carrot palette), with a `@media (prefers-color-scheme: dark)` block overriding the same variable names for dark mode. When adjusting colors, edit the variables rather than hardcoding new colors in rules further down the file.

### Layout patterns

- The menu section (`#menu`) is grouped into `.menu-category` blocks (e.g. Pastries, Cakes, Gift Ideas), each with a `.category-title` and a `.menu-grid` (3-column CSS grid) of `.dish` cards — a `.dish-shot` image (optionally flagged with a `.dish-badge`, e.g. "Sale") plus `.dish-copy` holding a `.dish-head` (name + price), a `.dish-desc`, and, for items with size/pack variations, a `.dish-variants` `<dl>` of `<dt>`label`</dt><dd>`price`</dd>` pairs (e.g. per-size or per-box pricing) — when present, `.dish-price` shows "From &#8369;X" using the lowest variant price rather than a slash-separated list, and the size/pack no longer belongs in `.dish-name`. A commented template in `index.html` above the first category shows the exact markup to duplicate when adding a menu item.
- Below the hero, a full-width `.showcase` section shows one photo (`.showcase-frame`) with an italic `.showcase-caption` underneath — it has no nav link or `id`.
- The `#gallery` section (replaces the old `#story` section) is a horizontally scrollable strip (`.gallery-scroll`, scroll-snap) of `.gallery-shot` images — no captions, just photos.
- The order section (`#order`) uses a two-column "ticket" (`.ticket`) of `.contact-card`s. The "Text to Order" flow is a `<details>`/`<summary>` element (`.chip-expand`) that expands in place to show ordering steps — no JS needed.
- Responsive breakpoints are at `860px` (stacks hero/ticket to one column, `.menu-grid` to 2 columns) and `480px` (hides the `#gallery` nav link, `.menu-grid` to 1 column) — see the media queries at the bottom of `css/style.css`.

### Content notes

- Business contact info (phone, email, social links, order form, map) lives inline in the `#order` section of `index.html`. Order-flow instructions (text-to-order steps, payment details) are also inline there — update in place if the bakery's process changes.
- Copy uses HTML entities for typographic characters (`&rsquo;`, `&mdash;`, `&ntilde;`, `&#8209;` for non-breaking hyphen, `&Prime;` for the inch mark after cake sizes) rather than raw Unicode — follow this convention when editing text.
- Menu prices are HTML entities too (`&#8369;` for the peso sign, e.g. `&#8369;1,150`) — keep new `.dish-price` values in that format.
