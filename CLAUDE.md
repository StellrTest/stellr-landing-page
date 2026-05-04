# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static landing page for Stellr Media — a done-for-you digital marketing service for real estate agents. Conversion-optimized sales funnel driven by paid ads. Primary above-the-fold element is a VSL (video sales letter); conversion goal is a GHL strategy call booking.

Deployed to GitHub Pages: `https://stellrtest.github.io/stellr-landing-page/`

## Local Development

No build system. Serve files directly:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

## Files

- `index.html` — the entire landing page (inline CSS + HTML + JS)
- `privacy.html` — Privacy Policy page
- `terms.html` — Terms of Service page
- `logo.png` — Stellr serif wordmark (black on white square PNG, no transparency); referenced as `<img src="logo.png">` inside `.logo a` in the nav; inverted to white in the footer via `filter:brightness(0) invert(1)`

## Architecture

Everything in `index.html` is inline — `<style>`, HTML, and `<script>`. No build step, no bundler, no framework. The only external dependencies are Google Fonts (Inter) via CDN and the GHL calendar embed script.

**Page sections in order:** scroll-progress bar → urgency banner → sticky nav → hero (VSL + market checker) → testimonials + screenshot placeholders → stats bar → pain points → solution/services → case studies → how it works → guarantee → FAQ → booking (scarcity bar + GHL calendar) → footer → floating CTA button.

## Design System

- **Backgrounds:** sections alternate between `linear-gradient(160deg,#FFFFFF 0%,#F4F6F8 100%)` and the reverse
- **Accent color:** `#00A8E8` exclusively; secondary shade `#0086C0`; gradient `linear-gradient(135deg,#00A8E8,#0086C0)`
- **Glassmorphism cards:** `background:rgba(255,255,255,0.75)` + `backdrop-filter:blur(24px)` + `border:1px solid rgba(255,255,255,0.9)` — applied via `.card` class and directly on the VSL box
- **Buttons:** pill-shaped (`border-radius:9999px`), accent gradient fill, glint shimmer on hover via `::after` pseudo-element
- **Typography:** Inter, headings `font-weight:900; letter-spacing:-0.03em`
- **Dark surfaces** (VSL cover, footer): navy `#0A1628` / `#060E1A`

CSS variables in `:root` — always use them, never hardcode: `--accent`, `--accent-dark`, `--grad`, `--glass`, `--glass-border`, `--blur`, `--shadow`, `--shadow-lg`, `--r-pill`, `--r-card`.

## Animations & Interactions

**Fade-up (`.fu` class):** Elements start hidden (`opacity:0; transform:translateY(28px)`) and animate in when an `IntersectionObserver` (threshold `0.1`) fires, adding `.vis`. Stagger delay via `setTimeout(fn, i * 70)`.

**Counter animation:** Stat numbers use `data-target`, `data-prefix`, `data-suffix`, `data-decimal` attributes. A separate `IntersectionObserver` (threshold `0.5`) triggers `animateCount()` when scrolled into view. 1600ms cubic ease-out.

**Nav positioning:** `setNavTop()` sets nav `top` to `banner.offsetHeight` on load and `resize`, preventing the fixed nav from overlapping the urgency banner on mobile where it wraps to two lines.

**Sticky nav background:** Adds `.on` (frosted glass) when `scrollY > 60`.

**VSL click-to-play + post-roll:** Clicking the cover sets `iframe.src` with `?autoplay=1&enablejsapi=1`. A `window.message` listener watches for YouTube `onStateChange` event with `info:0` (ended) — when fired, shows `.vsl-end` overlay and auto-scrolls to `#booking` after 2.8s. Replace `VSL_ID = 'YOUR_YOUTUBE_VIDEO_ID'` before going live.

**Market availability checker:** Input in the hero lets visitors enter a city/zip. `checkMarket()` always returns a green "available" result after a 0.9s fake-check delay. Enter key supported.

**Scarcity bar:** `.scarcity-fill` animates to 73% width when scrolled into view via its own `IntersectionObserver`.

## Mobile

Primary traffic is mobile (paid ads). Key rules:
- `body { max-width:100vw; overflow-x:hidden }` — prevents horizontal scroll
- `.logo img { height:40px }` at `@media(max-width:768px)`
- Section padding: `72px` desktop → `56px` tablet → `48px` mobile

## GHL Calendar Embed

The booking section uses a live GoHighLevel calendar iframe:
```html
<iframe src="https://link.stellrmedia.com/widget/booking/kfFLUoVZZRwKFwkiCyZO"
        style="width:100%;min-height:700px;border:none;overflow:hidden"
        scrolling="no" id="2frEogycf0JcnEwgbcWe_1777927548215"></iframe>
<script src="https://link.stellrmedia.com/js/form_embed.js" type="text/javascript"></script>
```
To update the calendar, replace the `src` URL and `id` attribute with the new embed values from GHL.

## Screenshot Placeholders

The testimonials section has three `.shot-card` elements with `.shot-ph` placeholder divs. To replace with a real screenshot:
```html
<!-- Before -->
<div class="shot-ph">...</div>
<!-- After -->
<img src="screenshot1.png" alt="Client result">
```

## Remaining Placeholder Before Go-Live

- **VSL video:** `const VSL_ID = 'YOUR_YOUTUBE_VIDEO_ID'` in the script block
