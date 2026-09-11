# Week Audit - production site

Marketing site for Simplify Automation. One page, one file. `index.html` holds
the markup, the design-system CSS and ~55 lines of JS. No build step, no
framework, nothing to install. Drop the folder on any static host (GitHub Pages,
Netlify, Cloudflare Pages, Vercel, plain nginx) and it is live.

```
index.html    the whole site
robots.txt    crawler rules - see the sitemap note below
assets/       put jadon.jpg here (see "Image still needed")
```

## Positioning

Written for **any local business owner losing hours to repeat work** - trades,
shops, offices, nonprofits. Restoration appears exactly once, low on the page in
the "Who I am" section, as experience rather than a gate:

> I work mostly with home service and restoration companies. But if you are a
> local business owner losing hours to repeat work, come talk to me.

Rules the copy follows, so later edits stay consistent:

- Lead with the owner's week, never the technology. No section opens with "AI",
  "automation", "agents" or "workflows". Those are the mechanism, not the promise.
- Pain is concrete but not industry-gated. "Calls that went to voicemail after
  5pm" rather than "missed water damage calls".
- Roughly 6th grade reading level. Short sentences. No corporate abstraction and
  nothing like "streamline your operations".
- No em dashes anywhere in the file, code comments included.
- The CTA offers a conversation about where their week goes, never a demo.

The layout, typography and spacing come from the approved `Week Audit - Homepage`
design. Changed from it, all on request: the accent red was darkened for
contrast, the copy was rewritten twice (60 minute free call, no guarantee, no
pricing, broadened past restoration), the old section 04 was cut, and the brand
lockup is now "Jadon - Simplify Automation".

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
  `{ name, company, phone }`. Formspree, Basin, Netlify Forms, a Zapier or Make
  webhook, or your own endpoint all work.
- **`contactEmail`** - the fallback. If the endpoint is missing or errors, the
  form shows a `mailto:` link pre-filled with the lead's details, so a
  submission is never silently lost.

Until `formEndpoint` is set, **the form cannot deliver a lead.** It falls back to
the email link rather than pretending to succeed.

### 2. Real domain

Replace `https://example.com/` in `index.html` (`<link rel="canonical">` and
`og:url`) and in `robots.txt`.

### 3. Image still needed

| Where | What | Status |
| --- | --- | --- |
| Section 07, *Who I am* | `assets/jadon.jpg` - a real photo, not stock | **Not supplied** |

The markup is already wired. Save the file to that exact path and it replaces the
placeholder on load. If the file is missing the placeholder stays put, so the
page never shows a broken image. Both states are tested.

Once the real file is in, add `width` and `height` attributes to the `<img>`
matching its pixel size, so nothing shifts as it loads.

### 4. Sitemap line

`robots.txt` points at `sitemap.xml`, which does not exist. For a one page site a
sitemap earns nothing. Either delete that line or add a two line sitemap.

### 5. Optional

- **Social share image** - add `<meta property="og:image" content="...">` beside
  the other `og:` tags. Without one, shared links show text only.
- **Analytics** - nothing is loaded today. One script tag before `</body>`.
- **Structured data** - left out rather than filled with invented details. Add a
  `ProfessionalService` JSON-LD block once the legal name and phone are settled.

## Claims on the page, worth a re-read

These are load bearing and each appears in more than one place:

- The Week Audit is **60 minutes** and **free**. Nothing to buy on the call.
- The page does **not** promise the owner keeps the map. It says the map gets
  built on the call and walked through together.
- **No prices and no dollar figures anywhere.** Section 05 says pricing depends
  on what we find and gets quoted after the audit.
- **No named clients, no logo wall, no testimonials.** Section 06 is about what
  you look for and what you build, not who you have done it for.
- **"Most owners get back 10 or more hours in the first month"** and **"most are
  running in under two weeks"** are the two numeric claims. Make sure you can
  stand behind both.

## What was verified

Headless Chrome with real device emulation:

- **Contrast: zero failures.** Every piece of text meets WCAG AA. Accent is
  `#b31b1b` at 6.08:1 against the page background, with `#a51818` (6.85:1) for
  button hover and `#961515` (7.78:1) for the active state and small kickers.
  The darker steps are the base red scaled in linear RGB, so hue is identical
  and hover always gets darker.
- **Layout** at 320, 390, 768, 1024, 1280 and 1440 px. No horizontal overflow at
  any width, no stranded card in the four-up grids.
- **CTAs** - all five in-page anchors resolve to a real target.
- **Form** - empty submit blocked; configured endpoint shows the success panel
  and moves focus to it; endpoint error re-enables the button and offers the
  email fallback; honeypot silently discards bots.
- **Photo** - tested with the file present and absent.
- **Accessibility** - labels, heading order (H1 to H2 to H3, no skips),
  landmarks, alt text, focus rings, skip link, `prefers-reduced-motion`.

## Performance

- One HTML file, all CSS inline. One render-blocking request.
- Archivo loads from Google Fonts asynchronously with `display=swap` and a
  `<noscript>` fallback, so text paints immediately in the system fallback.
- No JavaScript libraries. About 40 KB, roughly 11 KB gzipped.

## Local preview

```bash
python -m http.server 8124 --directory "C:/Users/jadon/Documents/week-audit-site"
```
