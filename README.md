# Week Audit — production site

One page, one file. `index.html` contains the markup, the design-system CSS and
~60 lines of JS. No build step, no framework, no dependencies to install. Drop
the folder on any static host (Netlify, Cloudflare Pages, Vercel, S3, plain
nginx) and it is live.

```
index.html    the whole site
robots.txt    crawler rules — update the Sitemap line with the real domain
assets/       drop the two real images here (see "Placeholders" below)
```

The design system (tokens, type scale, spacing, components) is copied verbatim
from the approved `Week Audit - Homepage` design. Colors, typography, spacing,
layout, hierarchy, CTAs and responsive behaviour all match it.

---

## Before launch — required

Everything below is a placeholder. The site works without them, but leads will
not reach you until the first two are done.

### 1. Wire up the booking form

At the bottom of `index.html`, in the `CONFIG` block:

```js
var CONFIG = {
  formEndpoint: "",              // ← REQUIRED
  contactEmail: "you@example.com", // ← REQUIRED
  waiveMonth: "October"
};
```

- **`formEndpoint`** — a URL that accepts a JSON `POST` of
  `{ name, company, phone }`. Any form backend works: Formspree, Basin,
  Netlify Forms, a Zapier/Make webhook, or your own endpoint.
- **`contactEmail`** — the fallback. If the endpoint is missing or returns an
  error, the form shows a message with a `mailto:` link that pre-fills the
  lead's details, so a submission is never silently lost.
- **`waiveMonth`** — updates every "$500, waived through …" mention on the page
  at once (offer section, step 1, pricing table, FAQ).

### 2. Real domain

Replace `https://example.com/` in three places near the top of `index.html`
(`<link rel="canonical">`, `og:url`) and in `robots.txt`.

### 3. Placeholders — images

Two hatched placeholder boxes are in the page. Each is preceded by an HTML
comment with the exact `<img>` tag to paste in its place:

| Where | What's needed |
| --- | --- |
| Section 04, *What the map looks like* | An anonymized sample Week Audit map |
| Section 08, *Who I am* | A real photo of you — not stock |

Both sit inside `.grayscale` frames, matching the design.

### 4. Optional

- **Social share image** — add `<meta property="og:image" content="...">` next
  to the other `og:` tags. Without one, shared links show text only.
- **Analytics** — nothing is loaded today. Add a single script tag before
  `</body>` if you want it.
- **Structured data** — deliberately omitted rather than filled with invented
  business details. Add a `ProfessionalService` JSON-LD block once the legal
  name, address and phone are settled.

---

## Two things to decide

**1. The page says 90 minutes; the brief said a 15-minute call.**
The approved design commits to a 90-minute Week Audit in eleven places — the
hero, the offer, the guarantee, the pricing table, the FAQ ("I don't have 90
minutes"), the sticky bar. It is the offer, not a detail. The build keeps the
design as approved. If the call really is 15 minutes, that is a copy decision
across the whole page, not a find-and-replace — say the word and I'll rework it.

**2. Text on the brand red falls below WCAG AA.**
`--color-bg` on `--color-accent` measures **3.76:1**; AA wants 4.5:1 for text
this size. It affects the three primary CTA buttons and all the supporting copy
inside the red booking section. This comes straight from the approved palette,
so nothing was changed. Options, if you want it addressed:

- Darken the red *behind small text* to `--color-accent-700` (`#ae1800`) →
  6.5:1. Buttons and the booking panel read slightly deeper.
- Leave it. It is a common brand trade-off, and the rest of the page
  (body copy, links, kickers, table) passes comfortably.

Nothing else in the audit failed: every input is labelled, the heading outline
runs H1 → H2 → H3 with no skips, landmarks are in place, no duplicate IDs, no
console errors.

---

## What was verified

Tested in headless Chrome with real device emulation:

- **Layout** at 320, 390, 768, 1024, 1280 and 1440 px — no horizontal overflow
  at any width, and no stranded card in the four-up grids.
- **CTAs** — all five in-page anchors resolve to a real target.
- **Form** — empty submit is blocked; a configured endpoint shows the success
  panel and moves focus to it; an endpoint error re-enables the button and
  offers the email fallback; the honeypot field silently discards bots.
- **Accessibility** — labels, heading order, landmarks, alt text, focus rings,
  skip link, `prefers-reduced-motion`.

## Performance notes

- One HTML file, all CSS inline — one render-blocking request, no CSS round trip.
- Archivo is loaded from Google Fonts asynchronously with `display=swap` and a
  `<noscript>` fallback, so text paints immediately in the system fallback.
- No JavaScript libraries, no images above the fold, no web fonts blocking
  render. The whole document is ~45 KB before compression.
- When you add the two real images, export them at 2× the display size, save as
  WebP or optimized JPEG, and keep the `width`/`height` attributes in the
  suggested `<img>` tags so nothing shifts as they load.

## Local preview

```bash
python -m http.server 8124 --directory "C:/Users/jadon/Documents/week-audit-site"
```
