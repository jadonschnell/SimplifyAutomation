# Week Audit - production site

Marketing site for Simplify Automation. One page, one file. `index.html` holds
the markup, the design-system CSS and ~55 lines of JS. No build step, no
framework, nothing to install. Drop the folder on any static host (GitHub Pages,
Netlify, Cloudflare Pages, Vercel, plain nginx) and it is live.

```
index.html    the whole site
robots.txt    crawler rules - see the sitemap note below
assets/       jadon-photo.jpg, the headshot
```

## Positioning

Written for **any local business owner losing hours to repeat work** - trades,
shops, offices, nonprofits. No industry is named anywhere on the page. The one
specificity line that used to sit in "Who I am" was removed on request, so
nothing signals that some businesses are a better fit than others.

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

### 1. Booking form - done

The form posts to **Web3Forms**, which delivers each submission to the inbox
tied to the access key. Nothing left to configure.

```js
var CONFIG = {
  formEndpoint: "https://api.web3forms.com/submit",
  accessKey: "beb63cd3-a461-4859-85d5-216a87a10549",
  contactEmail: "jadonschnell@gmail.com"
};
```

The access key is a public submission token, not a secret. It is meant to sit in
client-side HTML, and it is also a hidden input in the form itself.

Three layers, so a lead is hard to lose:

1. **With JavaScript** the submit handler POSTs JSON and shows the inline
   success panel without leaving the page.
2. **Without JavaScript** the form's own `action` and `method` post straight to
   Web3Forms, which shows its default thank-you page. The browser validates the
   required fields natively, since the form carries no `novalidate`.
3. **If the request fails** the button re-enables and the error offers a
   `mailto:` link to `jadonschnell@gmail.com`, pre-filled with the lead's name,
   company and phone. That address is visible in the page source and will be
   picked up by scrapers; swap it for a role address if that becomes a problem.

Two honeypots catch bots: a hidden text field, and Web3Forms' own `botcheck`.
Both are inside an `aria-hidden` container with `tabindex="-1"`, so neither is
reachable by keyboard or screen reader.

Verified end to end against the live API: HTTP 200, `success: true`, with name,
company and phone delivered and the subject set to the company name.

### 2. Real domain

Replace `https://example.com/` in `index.html` (`<link rel="canonical">` and
`og:url`) and in `robots.txt`.

### 3. Photo - done

`assets/jadon-photo.jpg`, shown in section 07. The source was a 1.8 MB PNG;
it ships as an optimised 1100px JPEG at 89 KB, with `width` and `height` set so
nothing shifts while it loads. The design's grayscale treatment was removed on
request, so it renders in colour.

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
- **No industry is named.** The page reads as open to any local business owner.
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
- **Form** - seven cases: empty submit blocked, the no-JS path's action and
  access key, success panel and focus, network failure recovery, Web3Forms
  answering `success: false`, honeypot, and a live end-to-end submission
  against the real API.
- **Photo** - loads at 1100px, renders at 520px, no filter applied.
- **Accessibility** - labels, heading order (H1 to H2 to H3, no skips),
  landmarks, alt text, focus rings, skip link, `prefers-reduced-motion`.

## Performance

- One HTML file, all CSS inline. One render-blocking request.
- Archivo loads from Google Fonts asynchronously with `display=swap` and a
  `<noscript>` fallback, so text paints immediately in the system fallback.
- No JavaScript libraries. Page weight is about 99 KB: 9 KB of gzipped
  HTML plus the 89 KB photo.

## Local preview

```bash
python -m http.server 8124 --directory "C:/Users/jadon/Documents/week-audit-site"
```
