# SESSIONS.md

A running log of work sessions on this repo: what changed, why, and what's still open. Newest entry first. It records the decisions and context that commit messages don't. For how the code works, see `CLAUDE.md`; for the visual system, see `DESIGN.md`.

**Adding an entry:** at the end of a session, add a section at the top with the date (YYYY-MM-DD), a short title, and these headings: *Done*, *Decisions*, *Open / next*. Keep it brief, link to files rather than repeating their contents, and move any resolved items out of earlier entries' *Open* lists.

---

## 2026-11-07: Square OG image

### Done
- `assets/og-image.jpg` is now **square, 1200×1200** (~120 KB). The layout is stacked and centered: brand, eyebrow, headline, lead, then the illustration with two float badges. The source is `assets/og-image.html`.
- Updated `og:image:width`/`height` in `index.html` and the skill starter, plus the dimensions and regenerate command in `CLAUDE.md`, `GEMINI.md`, `AGENTS.md`, and `DESIGN.md`.

### Decisions
- Square by request; WhatsApp shows square link thumbnails well. Platforms that force 1.91:1 (Facebook, LinkedIn, X) centre-crop it to about y 286–914, so the headline and lead stay in that band, and the brand and most of the illustration are cut off there.

### Open / next
- [ ] Check the preview in WhatsApp and the Facebook Sharing Debugger after the deploy. If the landscape crops look poor, add a separate 1200×630 image for `twitter:image`.

---

## 2026-11-06 — Docs, design system, favicon and social previews

Commit: `4f469af`

### Done
- **`CLAUDE.md`**: architecture notes for the single-file site: CSS-variable theming, the data-attribute-driven rules section, hard-coded rule counts, and the GitHub Pages deploy.
- **`GEMINI.md`** and **`AGENTS.md`**: the same guidance for Gemini CLI and for other agents (Codex, Cursor, Copilot, and others). These are separate copies, not links.
- **Skill `.claude/skills/bong-adda-design/`**: `SKILL.md` (workflow, design rules, component catalog, checklist), `assets/starter.html` (a working page skeleton with `{{PLACEHOLDER}}`s), and `assets/design-system.css` (the stylesheet copied exactly from `index.html`).
- **`DESIGN.md`**: the full design spec: tokens for both themes, type scale, shape and shadow, motion, background stack, components, breakpoints, voice, illustration, icon and OG card, and accessibility.
- **Favicon set** in `assets/`: `favicon.svg` (a vector redraw of `.brand-icon` with a clay bhar), `favicon-32.png`, and `apple-touch-icon.png`.
- **Social previews**: `assets/og-image.jpg` (originally 1200×630, then portrait 1080×1350, now square; see 2026-11-07), rendered with headless Chrome from `assets/og-image.html`. Open Graph, Twitter card, and canonical tags were added to `index.html` `<head>` and as placeholders in the skill's starter.

### Decisions
- `DESIGN.md` is the design source of truth. `index.html` wins if they disagree, and then `DESIGN.md` gets updated.
- The favicon is drawn as vector art rather than using the ☕ emoji, which renders differently on every OS.
- The OG image is a JPEG (~125 KB; a PNG would be about 4× larger) to stay under WhatsApp's ~300 KB preview limit, since WhatsApp is the main place links get shared.
- Pages stay single self-contained HTML files with no build step, and the only external dependency is Google Fonts.
- All pages share the `localStorage` theme key `ba_theme`.

### Open / next
- [ ] After the deploy, check the link preview in WhatsApp and the Facebook Sharing Debugger (it also clears cached previews).
- [ ] The OG and canonical URLs assume `https://d4rkr0n1n-playground.github.io/bong-adda/`. Update `index.html`, the starter, and `CLAUDE.md` if a custom domain is added.
- [ ] The deploy uploads the whole repo root, so `.claude/`, the `*.md` docs, and `assets/og-image.html` are publicly served. Consider limiting the Pages artifact to `index.html` and `assets/` if that matters.
- [ ] The skill's `starter.html` and `design-system.css` are copies of `index.html`. Re-sync them after any style change, and update `DESIGN.md` too.
- [ ] `CLAUDE.md`, `GEMINI.md`, and `AGENTS.md` are three copies of the same guidance and must be kept in sync. To consolidate, make `AGENTS.md` canonical: reduce `CLAUDE.md` to the single line `@AGENTS.md`, and set Gemini's `context.fileName` to `AGENTS.md`.
- [ ] `README.md` is still a one-line stub.
