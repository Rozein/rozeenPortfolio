# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-page personal CV/portfolio website for Rozeen Nakarmi, a Senior Software Engineer. The entire site lives in one file: `index.html`.

## Development

No build system, package manager, or framework — it's plain HTML, CSS, and vanilla JavaScript. To preview:

```bash
open index.html          # macOS
python3 -m http.server   # or serve locally at http://localhost:8000
```

There are no lint, test, or CI commands.

## Architecture

**Single-file structure** (`index.html`):
- `<head>` — meta tags, SEO (Open Graph, Twitter Card, JSON-LD structured data), Google Fonts (`Instrument Serif`, `DM Sans`, `JetBrains Mono`), and all CSS in a `<style>` block
- `<body>` — semantic HTML sections followed by a `<script>` block at the bottom

**CSS design system** (CSS custom properties in `:root`):
- `--bg`, `--bg-elevated`, `--bg-card`, `--surface` — dark background layers
- `--accent: #c4f04d` — lime green accent (used for highlights, CTAs, borders)
- `--serif`, `--sans`, `--mono` — font stacks
- `--border`, `--border-light` — border colors

**Page sections** (in order): Hero → About → Skills → Projects → Experience → Education → Contact → Footer

**Reveal animation system**: Elements with class `reveal` are initially hidden (opacity 0, translateY). An `IntersectionObserver` adds class `visible` when they scroll into view. Stagger delays are applied via `stagger-1` through `stagger-6` classes.

**JavaScript (inline `<script>` at bottom)**:
- Scroll reveal via `IntersectionObserver`
- Mobile hamburger menu toggle
- Active nav link highlighting via `IntersectionObserver` on sections
- Animated stat counters (counts up on first viewport entry)
- Consolidated scroll handler: nav compact-on-scroll, scroll progress bar, back-to-top visibility
- All animations respect `prefers-reduced-motion`

## Conventions

- Max content width: `1200px`, centered with `margin: 0 auto`
- Section padding: `7rem 3rem` desktop, `4rem 1.5rem` mobile (`≤768px`)
- Grids collapse from multi-column to single column at `≤768px`
- Touch targets min `48px` height enforced via `@media (pointer: coarse)`
- Inline SVG icons (no icon library dependency)
