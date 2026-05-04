# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file static landing page (`index.html`) for Stellr Media — a real estate agent marketing service. Conversion-optimized sales funnel targeting real estate agents via paid ads. Primary above-the-fold element is a VSL (video sales letter); conversion goal is a Calendly strategy call booking.

Deployed to GitHub Pages: `https://stellrtest.github.io/stellr-landing-page/`

## Local Development

No build system. Serve the file directly:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

## Architecture

Everything lives in `index.html` — inline CSS (`<style>`), HTML, and inline JavaScript (`<script>`). The only external dependency is Google Fonts (Inter) via CDN.

**Page sections in order:** scroll-progress bar → urgency banner → sticky nav → hero (VSL) → stats bar → pain points → solution/services → case studies → how it works → testimonials → guarantee → FAQ → booking (Calendly) → footer → floating CTA button.

Assets: `logo.png` — the Stellr serif wordmark (black on white square PNG, no transparency). Referenced as `<img src="logo.png">` inside `.logo a` in the nav.

## Design System

- **Backgrounds:** sections alternate between `linear-gradient(160deg,#FFFFFF 0%,#F4F6F8 100%)` and the reverse
- **Accent color:** `#00A8E8` exclusively; secondary shade `#0086C0`; gradient `linear-gradient(135deg,#00A8E8,#0086C0)`
- **Glassmorphism cards:** `background:rgba(255,255,255,0.75)` + `backdrop-filter:blur(24px)` + `border:1px solid rgba(255,255,255,0.9)` — applied via `.card` class and directly on the VSL box
- **Buttons:** pill-shaped (`border-radius:9999px`), accent gradient fill, glint shimmer on hover via `::after` pseudo-element
- **Typography:** Inter, headings `font-weight:900; letter-spacing:-0.03em`
- **Dark surfaces** (VSL cover, footer): navy `#0A1628` / `#060E1A`

CSS variables are defined in `:root` — always use them rather than hardcoding values: `--accent`, `--accent-dark`, `--grad`, `--glass`, `--glass-border`, `--blur`, `--shadow`, `--shadow-lg`, `--r-pill`, `--r-card`.

## Animations & Interactions

**Fade-up (`.fu` class):** Elements start hidden (`opacity:0; transform:translateY(28px)`) and animate in when an `IntersectionObserver` (threshold `0.1`) fires, adding the `.vis` class. Stagger delay applied via `setTimeout(fn, i * 70)`.

**Counter animation:** Stat numbers use `data-target`, `data-prefix`, `data-suffix`, `data-decimal` attributes. A separate `IntersectionObserver` (threshold `0.5`) triggers `animateCount()` on each element when scrolled into view. The animation runs over 1600ms with cubic ease-out.

**Nav positioning:** The nav `top` is set dynamically via JS (`setNavTop()`) to `banner.offsetHeight` on load and on `resize`. This prevents the fixed nav from overlapping the urgency banner, which wraps to two lines on mobile. CSS sets `top:0` as the default; JS overrides it.

**Sticky nav background:** Adds class `.on` (frosted glass background) when `scrollY > 60`.

**VSL click-to-play:** Clicking the cover sets `iframe.src` to the YouTube embed URL with `?autoplay=1`. Replace `VSL_ID = 'YOUR_YOUTUBE_VIDEO_ID'` with the real video ID before going live.

## Mobile

Primary traffic is mobile (ads). Key mobile rules:
- `body { max-width:100vw; overflow-x:hidden }` — prevents horizontal scroll from any overflowing child
- `.logo img { height:40px }` at `@media(max-width:768px)`
- `.cal-zone code { word-break:break-all; overflow-wrap:break-word; white-space:normal }` — prevents the long Calendly URL from causing horizontal overflow

## Placeholder Replacements Before Go-Live

1. **VSL video:** `const VSL_ID = 'YOUR_YOUTUBE_VIDEO_ID'` in the script block
2. **Calendly widget:** Replace the `.cal-zone` div with the Calendly inline embed snippet:
   ```html
   <div class="calendly-inline-widget"
        data-url="https://calendly.com/YOUR_USERNAME/strategy-call"
        style="min-width:320px;height:700px;"></div>
   <script src="https://assets.calendly.com/assets/external/widget.js" async></script>
   ```
