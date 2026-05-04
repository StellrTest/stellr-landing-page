# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file static landing page (`index.html`) for Stellr Media — a real estate agent marketing service. The page is a conversion-optimized sales funnel targeting real estate agents via paid ads, with a VSL (video sales letter) as the primary above-the-fold element and a Calendly booking widget as the conversion goal.

## Local Development

No build system. Serve the file directly:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

## Architecture

Everything lives in `index.html` — inline CSS (`<style>`), HTML, and inline JavaScript (`<script>`). There are no external dependencies beyond Google Fonts (Inter) loaded via CDN.

**Page sections in order:** urgency banner → sticky nav → hero (VSL) → stats bar → pain points → solution/services → case studies → how it works → testimonials → guarantee → FAQ → booking (Calendly) → footer.

## Design System

The design system is defined in `SKILL.md` (module index) with individual `.md` files for each component. **Read the relevant SKILL.md modules before making any visual changes.**

Key rules enforced by the design system:
- **Backgrounds:** monochrome white/light-gray gradients — sections alternate between `linear-gradient(160deg,#FFFFFF 0%,#F4F6F8 100%)` and the reverse. Never flat single colors.
- **Accent color:** `#00A8E8` exclusively — used for buttons, eyebrows, gradient text, icons, borders, checkmarks, the progress bar. Secondary shade `#0086C0`.
- **Glassmorphism:** all cards use `background:rgba(255,255,255,0.75)` + `backdrop-filter:blur(24px)` + `border:1px solid rgba(255,255,255,0.9)`.
- **Buttons:** pill-shaped (`border-radius:9999px`), gradient fill `linear-gradient(135deg,#00A8E8,#0086C0)`, glint shimmer on hover via `::after` pseudo-element.
- **Typography:** Inter font, headings use `font-weight:900; letter-spacing:-0.03em`.
- **Dark surfaces** (VSL cover, footer): navy `#0A1628` / `#060E1A` — not pure black, not purple.

## Placeholder Replacements

Two things must be swapped before the page goes live:

1. **VSL video** — find `const VSL_ID = 'YOUR_YOUTUBE_VIDEO_ID'` in the script block and replace with the actual YouTube video ID.
2. **Calendly booking widget** — replace the `.cal-zone` div with the Calendly inline embed snippet:
   ```html
   <div class="calendly-inline-widget"
        data-url="https://calendly.com/YOUR_USERNAME/strategy-call"
        style="min-width:320px;height:700px;"></div>
   <script src="https://assets.calendly.com/assets/external/widget.js" async></script>
   ```

## CSS Conventions

CSS variables are defined in `:root` — always use them rather than hardcoding color values. The key variables: `--accent`, `--accent-dark`, `--grad`, `--glass`, `--glass-border`, `--blur`, `--shadow`, `--shadow-lg`, `--r-pill`, `--r-card`.

Fade-in animations use the `.fu` class (opacity:0 + translateY) toggled to `.fu.vis` via IntersectionObserver. Counter animations on stat numbers are driven by `data-target`, `data-prefix`, `data-suffix`, and `data-decimal` attributes.
