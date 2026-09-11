# Week Audit - production site

One page, one file. `index.html` contains the markup, the design-system CSS and
~55 lines of JS. No build step, no framework, no dependencies to install. Drop
the folder on any static host (Netlify, Cloudflare Pages, Vercel, GitHub Pages,
plain nginx) and it is live.

```
index.html    the whole site
robots.txt    crawler rules - update the Sitemap line with the real domain
assets/       drop the two real images here (see "Placeholders" below)
```

The layout, typography, spacing and structure come from the approved
`Week Audit - Homepage` design. Two things were deliberately changed from it,
both on request: the accent red was darkened for contrast, and the copy was
rewritten (60 minute call, free, simpler sentences, no em dashes, no build or
support pricing).

---

## Before launch - required

### 1. Wire up the booking form

At the bottom of `index.html`, in the `CONFIG` block:

```js
var CONFIG = {
  formEndpoint: "",                // <- REQUIRED
  contactEmail: "you@example.com"  // <- REQUIRED
};
```

- **`formEndpoint`** - a URL that accepts a JSON `POST` of
  `{ name, company, phone }`. Any form backend works: Formspree, Basin,
  Netlify Forms, a Zapier or Make webhook, or your own endpoint.
- **`contactEmail`** - the fallback. If the endpoint is missing or returns an
  error, the form shows a message with a `mailto:` link that pre-fills the
  lead's details, so a submission is never silently lost.

### 2. Real domain

Replace `https://example.com/` in three places near the top of `index.html`
(`<link rel="canonical">` and `og:url`) and in `robots.txt`.

### 3. Images still needed

Two hatched placeholder boxes are in the page. Each is preceded by an HTML
comment with the exact `<img>` tag to paste in its place.

| Where | What's needed | Status |
| --- | --- | --- |
| Section 04, *What the map looks like* | A sample Week Audit map, details anonymized | **Not supplied** |
| Section 08, *Who I am* | A real photo of you, not stock | **Not supplied** |

Both sit inside `.grayscale` frames, matching the design. The page looks
deliberate with the placeholders in place, but neither should ship that way.

### 4. Optional

- **Social share image** - add `<meta property="og:image" content="...">` next
  to the other `og:` tags. Without one, shared links show text only.
- **Analytics** - nothing is loaded today. Add a single script tag before
  `</body>` if you want it.
- **Structured data** - left out rather than filled with invented business
  details. Add a `ProfessionalService` JSON-LD block once the legal name,
  address and phone are settled.

---

## The offer, as the page now states it

Worth checking these read the way you actually sell, because they are load
bearing and they appear in several places each:

- The Week Audit is **60 minutes** and **free**. Nothing to buy on the call.
- The page does **not** promise the owner keeps the map. It says the map gets
  built on the call and walked through together.
- **No build or support prices anywhere.** Section 06 says pricing depends on
  what we find and that you will quote it after the audit. Step 2 says "Quoted
  after the audit"; Step 3 says "Monthly. Cancel anytime."
- The **$100 guarantee** is still on the page, in three places: the guarantee
  box, the FAQ, and the booking section. It is the only dollar figure left. Now
  that the call is free, decide whether paying $100 for a call that cost the
  owner nothing still reads the way you want.
- **Anissa Branch is named** as a real client, "used with permission." Confirm
  that permission is on file before this goes public.

---

## What was verified

Tested in headless Chrome with real device emulation:

- **Contrast: zero failures.** Every piece of text on the page meets WCAG AA.
  The accent is `#b31b1b` at 6.08:1 against the page background, with
  `#a51818` (6.85:1) for button hover and `#961515` (7.78:1) for the active
  state and the small uppercase kickers. The two darker steps are the base red
  scaled in linear RGB, so the hue is identical and hover always gets darker.
- **Layout** at 320, 390, 768, 1024, 1280 and 1440 px - no horizontal overflow
  at any width, and no stranded card in the four-up grids.
- **CTAs** - all five in-page anchors resolve to a real target.
- **Form** - empty submit is blocked; a configured endpoint shows the success
  panel and moves focus to it; an endpoint error re-enables the button and
  offers the email fallback; the honeypot field silently discards bots.
- **Accessibility** - labels, heading order (H1 -> H2 -> H3, no skips),
  landmarks, alt text, focus rings, skip link, `prefers-reduced-motion`.
- **No em dashes anywhere in the file**, including code comments.

## Performance notes

- One HTML file, all CSS inline - one render-blocking request, no CSS round trip.
- Archivo is loaded from Google Fonts asynchronously with `display=swap` and a
  `<noscript>` fallback, so text paints immediately in the system fallback.
- No JavaScript libraries, no images above the fold. The whole document is
  ~44 KB, about 11 KB gzipped.
- When you add the two real images, export them at 2x the display size, save as
  WebP or optimized JPEG, and keep the `width`/`height` attributes in the
  suggested `<img>` tags so nothing shifts as they load.

## Local preview

```bash
python -m http.server 8124 --directory "C:/Users/jadon/Documents/week-audit-site"
```
