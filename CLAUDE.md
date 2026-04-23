# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Follow My Leash is a single-page landing site for a pet-sitting business in Sector 4, Bucharest. There is no build step — the entire site is one file: `index.html`, styled with Tailwind CSS loaded from CDN.

## Stack

- `index.html` — the entire site: HTML structure, inline CSS, inline JavaScript, and Tailwind config
- `translations.json` — bilingual (EN/RO) copy for all user-facing strings; loaded at runtime via `fetch()`
- `public/` — images (logos, sitter photos)
- `vercel.json` — HTTP security headers for Vercel deployment
- `sitemap.xml`, `robots.txt`, `llms.txt` — SEO/discoverability files

No npm, no bundler, no framework, no `src/` folder.

## Development

Open `index.html` directly in a browser, or serve it locally:

```bash
npx serve .
# or
python3 -m http.server 8080
```

There are no tests, no lint scripts, and no build commands.

## Architecture

### Internationalisation

Language is toggled at runtime. `translations.json` holds all copy under `"en"` and `"ro"` keys. JavaScript fetches this file, then populates elements via `data-i18n` attributes. The active language is stored in `localStorage`. Romanian-only sections use the CSS class `.lang-ro { display: none }` toggled by JS.

### Booking Form

A 4-step wizard (`#slide-track` / `.step-slide`) advances via `next`/`back` buttons. State is tracked in JS variables (pet type, service, dates, contact details). On completion, a pre-filled WhatsApp URL is constructed and opened, with no server-side backend.

### Pricing Logic

Calculated client-side from base rates defined in JS constants. Overnight, multi-pet, and day-count discounts are applied inline before building the WhatsApp message.

### Scroll Reveal

Elements with `data-reveal` start hidden (`opacity:0; transform:translateY(28px)`) and animate in via an `IntersectionObserver` that adds `.is-visible`.

### Legal Modals

Privacy Policy and Terms are rendered as in-page modal overlays (`.modal-overlay` / `.modal-box`) toggled by JS — no separate pages.

## Deployment

Deployed to Vercel. `vercel.json` sets CSP and security headers. The canonical URL is `https://follow-my-leash.vercel.app/`.

## Key Business Details (for copy accuracy)

- Base rate: 60 RON/hour per pet; +30 RON/hour per additional pet
- Overnight flat rate: 350 RON (7+ hours); 15% discount for 5+ day bookings
- Contact WhatsApp: +40 772 159 680
- Service area: Sector 4, Bucharest (Tineretului, Berceni, Brâncuși)
