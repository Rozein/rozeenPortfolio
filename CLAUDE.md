# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-page personal CV/portfolio website for Rozeen Nakarmi, a Lead Software Engineer / Tech Lead. The entire site lives in one file: `index.html`.

## Development

No build system, package manager, or framework — it's plain HTML, CSS, and vanilla JavaScript. Animation libraries load from jsDelivr at the end of `<body>`. To preview:

```bash
open index.html          # macOS
python3 -m http.server   # or serve locally at http://localhost:8000
```

There are no lint, test, or CI commands.

## Architecture

**Single-file structure** (`index.html`):
- `<head>` — meta tags, SEO (Open Graph, Twitter Card, JSON-LD structured data), Google Fonts (`Archivo` variable with `wdth` axis, `IBM Plex Sans`, `IBM Plex Mono`), a tiny inline script that adds the `motion` class to `<html>` unless the user prefers reduced motion, and all CSS in a `<style>` block
- `<body>` — semantic HTML sections, then CDN scripts (GSAP 3.13 core, ScrollTrigger, SplitText, Lenis) and one inline `<script>` IIFE

**Visual direction**: an engineering spec sheet / systems ledger — warm paper, ink, one vermilion signal colour, hairline rules instead of cards, numbered sections, square (not rounded) shapes. Avoid reintroducing rounded cards, pill badges, gradients/glows or emoji icons.

**CSS design system** (custom properties in `:root`):
- `--paper`, `--paper-2` — page backgrounds; `--ink`, `--ink-2`, `--ink-3` — text from strongest to dimmest (all ≥4.5:1 on paper)
- `--rule` — hairlines; `--signal` (decorative/large text) and `--signal-ink` (small text on paper) — the only accent
- `--on-ink`, `--on-ink-2`, `--on-ink-rule` — colours for the inverted Contact section and footer
- `--display` (Archivo, set via `font-stretch` 72–125%), `--sans`, `--mono` — font stacks
- `--max`, `--gutter`, `--col-gap`, `--nav-h`, `--ease-out`

**Layout**: content sits in `.wrap` (max `1280px`); most blocks use a 12-column grid. Each section starts with `.sec-head` = `.sec-rule` (drawn line) + `.sec-meta` (number + kicker) + `.sec-title`.

**Page sections** (in order): Hero (with the animated "Fig. 01" request-path SVG) → stack marquee → About (01) → Skills (02, spec rows) → Projects (03, numbered work index) → Experience (04, journey with sticky dates and a scroll-filled rail) → Education (05) → Certifications (06) → Contact (07, inverted ink block) → Footer

**Motion system** (GSAP, only when `html.motion` is present):
- Elements GSAP reveals are hidden by CSS under `.motion`: `[data-reveal]`, `[data-split]`, `[data-hero]`, `.hero-meta > *`, `.stat-item`, `.hero-fig`; `.sec-rule` starts at `scaleX(0)`
- `[data-split]` headings get SplitText masked line reveals (`data-split="hero"` is the hero intro); `[data-reveal]` uses `ScrollTrigger.batch`
- If GSAP fails to load, or `init()` throws, the `motion` class is removed so all content is visible. Reduced-motion users never get the class: no Lenis, no animation
- Lenis drives smooth scrolling and feeds `ScrollTrigger.update`; in-page anchors go through `scrollToTarget()` so they work with or without Lenis

**JavaScript also handles**: Kathmandu local time in the hero meta, mobile full-screen menu (Esc closes, scroll lock), active nav link via `IntersectionObserver`, scroll progress bar, nav hide-on-scroll-down, back-to-top, stat count-up, and the figure's packet loop (paused when the hero is off screen).

## Conventions

- Keep content edits and design edits separate; content must match the resume (see memory/brief workflow)
- Section padding: `clamp(5rem, 10vw, 8.5rem)` vertical
- Breakpoints: `≤1080px` nav collapses to the overlay menu; `≤860px` grids go single column; `≤480px` small-phone tweaks
- Touch targets min `48px` height enforced via `@media (pointer: coarse)`
- Inline SVG icons only (no icon library dependency)
