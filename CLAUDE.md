# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Bangali Adda Lucknow — a single-page community playbook (rules, etiquette, pledge) for a WhatsApp group. It is a static site with no build step, package manager, linter, or test suite.

## Development & deployment

- Preview locally by opening `index.html` in a browser, or serve the folder (e.g. `python -m http.server`). Clipboard features (`navigator.clipboard`) need a secure context, so use `localhost` instead of `file://` when testing copy buttons.
- Every push to `main` deploys to GitHub Pages via `.github/workflows/static.yml`. It uploads the **whole repository root** as the site, so any file committed at the root is publicly served.

## Architecture

Everything lives in `index.html`: all CSS in one `<style>` block in `<head>`, then the markup, then one inline `<script>` IIFE at the end of `<body>`. The only external dependency is Google Fonts.

**Theming:** design tokens are CSS custom properties on `:root` (light). `[data-theme="dark"]` on `<html>` overrides the same variables. When you add a color, shadow, or decorative stroke, define it in **both** blocks and use `var(--…)` instead of a hard-coded value. The theme toggle saves the choice to `localStorage` under the key `ba_theme`.

**Rules section (`#rules`)** is driven by data attributes that the script reads:
- Each rule is an `<article class="rule">` with `data-category` (`respect` | `content` | `admin`), `data-tone` (card accent color), and `data-rule` (the plain-text string the "Copy rule" button copies, prefixed with `📌 *Bangali Adda Lucknow* —`).
- Filter buttons use `data-filter` values that match `data-category`. Search matches against the card's `textContent`.
- The rule counts are **hard-coded**: the "11 simple guidelines" intro text, and the `.filter-count` badges on each filter button. Update them by hand when you add, remove, or recategorize a rule. Also keep the `RULE NN` label, the `aria-label="Copy Rule N"`, and the `data-rule` text in sync with the visible card content.

**Script behaviors:** theme toggle, category filter plus search (`/` focuses search, and `#empty` with a reset button shows when nothing matches), copy-to-clipboard for rules and a WhatsApp intro template, a shared `#toast`, and a pledge toggle button (`aria-pressed`). The pledge is UI-only and stores nothing.

Fonts in use: Plus Jakarta Sans (body), Space Grotesk (display), Instrument Serif (italic accents), Space Mono (labels), and Hind Siliguri (Bengali text). These are exposed as `--font-*` variables.

## Design system

`DESIGN.md` is the source of truth for colors (both themes), typography, shape and shadow, motion, components, breakpoints, and voice. Follow it for any visual change, and update it whenever you change a token or component in `index.html`. To build new pages, use the `bong-adda-design` skill in `.claude/skills/`. Its `assets/starter.html` and `assets/design-system.css` are copies of `index.html` and need re-syncing after style changes.

## Social preview and icons

- `assets/favicon.svg` is the master icon: a sindoor tile with a brutal shadow and a clay bhar, matching `.brand-icon`. `favicon-32.png` and `apple-touch-icon.png` (180px, on a cream background) are rendered from it.
- `assets/og-image.jpg` (1200×630, ~90 KB; keep it under 300 KB or WhatsApp may skip the preview) is rendered from `assets/og-image.html`, which reuses the hero SVG from `index.html`.
- OG and Twitter image URLs in `<head>` are absolute and assume `https://d4rkr0n1n-playground.github.io/bong-adda/`. Update them if a custom domain is added.
- To regenerate after editing a source, run headless Chrome from the repo root (it needs network access for Google Fonts):
  ```sh
  CH="/c/Program Files/Google/Chrome/Application/chrome.exe"
  "$CH" --headless=new --hide-scrollbars --force-device-scale-factor=1 --virtual-time-budget=8000     --window-size=1200,630 --screenshot="$PWD/assets/og-image.jpg" "file:///$PWD/assets/og-image.html"
  "$CH" --headless=new --hide-scrollbars --force-device-scale-factor=1 --default-background-color=00000000     --window-size=32,32 --screenshot="$PWD/assets/favicon-32.png" "file:///$PWD/assets/favicon.svg"
  ```
  For `apple-touch-icon.png`, screenshot a 180×180 page with a `#faeed7` background and `favicon.svg` centered at 152px. Social platforms cache previews, so after deploying, re-scrape the page with a tool such as the Facebook Sharing Debugger.
