# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-page personal CV/portfolio website for Rozeen Nakarmi, a Lead Software Engineer / Tech Lead. The site is `index.html` plus self-hosted fonts in `fonts/`.

## Development

No build system, package manager, or framework: plain HTML, CSS, and vanilla JavaScript. Animation libraries (GSAP 3.13 + ScrollTrigger + SplitText, Lenis) load from jsDelivr at the end of `<body>`.

Preview over HTTP (opening the file directly can block the relative font files):

```bash
python3 -m http.server 8765 --bind 127.0.0.1   # then open http://127.0.0.1:8765
```

There are no lint, test, or CI commands.

## Design rules (from the `design-taste-frontend` skill)

- **No em-dashes or en-dashes anywhere visible.** Use commas, colons, parentheses, or a spaced hyphen for date ranges (`Aug 2026 - Present`).
- One accent (cobalt `--accent`), cool neutrals, automatic light/dark via `prefers-color-scheme`. Do not reintroduce warm cream/paper + clay/orange palettes.
- Shape lock: surfaces use `--r-surface` (20px); buttons and chips are full pills.
- One eyebrow-style label on the page (the hero availability chip). No numbered section labels, locale/time strips, scroll cues, or decorative dots.
- One label per CTA intent: "Get in Touch" (contact) and "View My Work" (portfolio).
- Icons are Phosphor (regular) SVG paths inlined from `@phosphor-icons/core`; do not hand-draw icons.
- No `window.addEventListener('scroll')`; use IntersectionObserver or ScrollTrigger.

## Architecture

**`<head>`**: meta/SEO (Open Graph, Twitter Card, JSON-LD), `@font-face` for Geist and Geist Mono, a tiny inline script that adds `motion` to `<html>` unless the user prefers reduced motion, and all CSS.

**Tokens** (`:root`, redefined under `prefers-color-scheme: dark`): `--bg`, `--bg-2`, `--surface`, `--text`, `--text-2`, `--text-3`, `--line`, `--accent`, `--accent-hover`, `--on-accent`, `--accent-soft`, `--live`, `--shadow`, plus `--sans`, `--mono`, `--max` (1280px), `--gutter`, `--nav-h`, `--r-surface`, `--ease-out`.

**Page sections** (each a different layout family): Hero (kinetic type, no asset) → Highlights (4 metric tiles) → About (`#about`, word-brightening lead + text and details card) → Skills (`#skills`, 5-cell bento) → Projects (`#projects`, cards; pinned horizontal pan at ≥1024px) → Experience (`#experience`, sticky role index + roles) → Education (`#education`) and Certifications (`#certifications`) as an asymmetric pair → Contact (`#contact`) → Footer.

**Motion** (only when `html.motion` is present):
- CSS hides `[data-hero]`, `[data-reveal]`, `[data-split]` under `.motion`; GSAP reveals them
- `[data-split]` headings get SplitText masked line reveals; `data-split="hero"` is the hero headline
- The Projects pan follows the canonical pattern: `start: 'top top'`, `pin: true`, `end: '+=' + distance`, `scrub: 1`, inside `gsap.matchMedia('(min-width: 1024px)')`
- If GSAP fails to load or `init()` throws, `motion` is removed and all content is visible

**Without motion or GSAP**: IntersectionObservers still drive nav state, back-to-top, active nav link and the active role in the Experience index.

## Conventions

- Content must match the resume; punctuation follows the no-dash rule above
- Breakpoints: `≤1023px` nav becomes the overlay menu, Projects pan becomes a grid, Experience index hides; `≤767px` single column; `≤420px` small-phone tweaks
- Touch targets ≥44–48px
