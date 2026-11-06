---
name: bong-adda-design
description: Build new pages, sections, or whole websites in the Bangali Adda Lucknow visual language (warm neo-brutalist cards, sindoor/turmeric/teal palette, Space Grotesk + Instrument Serif type, Bengali × Lucknawi cultural motifs, light/"Night Adda" dark theme). Use whenever creating or extending any Bangali Adda / bong-adda web page, event page, landing page, microsite, or UI component so it stays visually and behaviorally consistent with the existing site.
---

# Bangali Adda design system

The reference implementation is `index.html` at the root of the bong-adda repo. This skill packages its design system so new pages look like siblings of it, not cousins. When you're working inside the repo, `DESIGN.md` at the root is the full spec: exact light and dark values for every token, the type scale, motion timings, and component anatomy. Consult it whenever this file is less specific.

## Bundled files

| File | Use |
|---|---|
| `assets/starter.html` | Complete, working page skeleton: fonts, full stylesheet, background layers, sticky nav, hero, ticker, card section, CTA panel, footer, toast, theme-toggle script. `{{PLACEHOLDER}}` markers show what to replace. **Start every new page from this file.** |
| `assets/design-system.css` | The exact stylesheet from `index.html` (tokens, base, every component, breakpoints). Read it to look up a component's precise CSS, or inline it when you aren't starting from the starter. |

## Workflow

1. **Copy, don't rewrite.** Copy `assets/starter.html` to the target path, then replace every `{{…}}` placeholder (`grep -n "{{" file` must return nothing when you're done). Delete starter sections you don't need; don't restyle them.
2. **Compose from existing components** (catalog below) before writing any new CSS. Most needs are covered by `bento-card`, `mini-bento`, `rule`-style cards, `vibe-box`, `chat-bubble`, `pledge`, `media-note`.
3. **If a new component is truly needed**, build it from the recipe in "Design rules", add its CSS in the same `<style>` block under a `/* ===== SECTION NAME ===== */` banner comment, and add mobile overrides inside the existing `@media (max-width: 960px)` / `(max-width: 650px)` blocks, not new ones.
4. **Single-file, no build.** Keep each page self-contained: inline `<style>` in `<head>` and one inline `<script>` IIFE before `</body>`. The only external dependency is Google Fonts. No frameworks or CDNs.
5. Run the checklist at the end.

## Design rules

### Color: always via tokens
Never hard-code a color in a component. Use `var(--token)`. Every new token must be defined in **both** `:root` and `[data-theme="dark"]`.

| Role | Token | Light | Use for |
|---|---|---|---|
| Page background | `--bg-base` | `#faeed7` warm cream | body, inputs |
| Card surface | `--bg-card` / `--bg-card-solid` | translucent / `#fffdf8` | cards (translucent ones also get `backdrop-filter: blur(10–12px)`) |
| Glass | `--bg-glass` | | sticky nav only |
| Text | `--ink`, `--ink-soft`, `--muted` | `#1e1915` … | headings / body copy / captions |
| Borders | `--line-strong` (outlines), `--line` (dividers) | | |
| **Primary accent** | `--sindoor` (+ `-soft`) | `#e84e1b` vermilion | primary buttons, serif accents, hovers, selection |
| Highlight | `--turmeric` (+ `-soft`) | `#f9bf3b` | eyebrows, underline swash, dark-mode CTA, toast border |
| Positive / calm | `--teal` (+ `-soft`) | `#0f5e47` | stats, green flags |
| Heritage | `--royal-awadh` | `#8c2d4b` | sparing decorative use |
| Tint backgrounds | `--rose-soft`, `--sky-soft`, `--lavender-soft` | | card tones, icon tiles |

Fixed exceptions already in use (keep them): the dark ticker/toast `#1e1915` with cream text `#fff8ec`, the deep-teal pledge gradient `#10493a → #0a3328`, and white text on sindoor buttons.

### Typography
| Variable | Font | Role |
|---|---|---|
| `--font-display` | Space Grotesk 800 | h1/h2/h3/h4, big numbers, brand. Tight tracking (`-0.03em` to `-0.06em`) |
| `--font-serif` | Instrument Serif *italic* | **One accent word or phrase per heading**, wrapped in `<span class="serif-italic">`, colored sindoor |
| `--font-sans` | Plus Jakarta Sans | Body text, line-height ~1.6 |
| `--font-mono` | Space Mono 700 | Eyebrows, tags, labels, counters: UPPERCASE, letter-spacing `0.04–0.18em`, 11–12px |
| `--font-bn` | Hind Siliguri | Any Bengali script. Always add `lang="bn"` |

Sizes use `clamp()`. h1 is `clamp(54px, 7.6vw, 102px)`, section titles `clamp(34px, 4.8vw, 56px)`.

### The neo-brutalist card recipe
Every raised surface follows this:
```css
border: 2px solid var(--line-strong);      /* 1.5px for pills/chips */
border-radius: var(--radius-xl | --radius-lg | 999px);  /* 28 / 22 / pills */
box-shadow: var(--shadow-brutal);          /* hard 4px offset, no blur */
background: var(--bg-card-solid);          /* or a soft-tone → card gradient */
```
- **Hover** lifts up-left and grows the shadow: `transform: translate(-2px,-3px); box-shadow: var(--shadow-brutal-lg);` with the springy easing `cubic-bezier(0.34, 1.56, 0.64, 1)`.
- **Tone variants** use a soft tint fading into the surface: `linear-gradient(160deg, var(--teal-soft) 0%, var(--bg-card-solid) 55%)`.
- Small icon tiles: 44px square, radius 12–13px, 1.5px border, `2px 2px 0` shadow, emoji centered.
- Everything interactive is a pill (`border-radius: 999px`).
- Dashed `--line` dividers separate card headers and footers.

### Layout
- Content width: wrap containers in `.wrap` (`min(1180px, 100% - 40px)`).
- Section rhythm: `padding: ~10px 0 84px`. Each section opens with `.section-header-block` → `.eyebrow` + `h2.section-title` (with a serif-italic accent).
- A full-bleed band (like `.rules-section`) uses `--bg-elevated` with `border-block: 2px solid var(--line-strong)`, and puts `.wrap` inside it.
- Grids: 2 or 3 columns with a 20–22px gap. They collapse to 1 column (or 3→2→1) at the 960px and 650px breakpoints.

### Voice and content
Bilingual and playful: English with Bengali (and occasional Hindi/Urdu) phrases like *Nomoshkar*, *Adaab*, *adda*, *para*, *আড্ডা চলুক*. Gen-Z-friendly but warm ("vibe check", "+100 Aura", "W Post"). Use plenty of emoji as icons, and mark decorative emoji `aria-hidden="true"`. Use the separators `✳`, `✦` and `·`. Cultural references blend Bengal (Howrah Bridge, alpana, dhak, cha in a bhar, Pujo) with Lucknow (Rumi Darwaza, Imambara, chikankari, tehzeeb, Gomti).

### Illustration
Use original inline SVG only (no external images). Thick `#29251f` strokes (3–6px), flat warm fills (`#ffc65c`, `#bf5b31`, `#e84e1b`, `#176c55`, `#f7a6a4`), dashed accent strokes, and sparkle stars. Give the wrapper `role="img"` with a descriptive `aria-label`, and set `aria-hidden="true"` on the inner `<svg>`. Background-layer SVGs should use the CSS variables (`--alpana-stroke`, `--skyline-*`) so they re-theme in dark mode.

## Component catalog (all in `design-system.css`)

| Component | Classes | Notes |
|---|---|---|
| Background stack | `.bg-stage` `.bg-pattern` `.lucknawi-canopy` `.cultural-float-layer` `.alpana-watermark` `.cultural-horizon` `.grain` | Fixed, `aria-hidden`, negative z-index. Keep them all on every page (already in the starter) |
| Sticky nav | `.header-sticky > header > nav.nav` with `.brand` (`.brand-icon` `.brand-bn`), `.nav-links`, `.nav-actions` (`.status-pill` `.theme-btn#themeToggle` `.small-btn`) | Links, status pill and Bengali brand tag hide below 960px |
| Hero | `.hero.wrap` (2-col) → `.eyebrow`, `h1` + `.serif-italic`, `.lead`, `.tldr-strip > .tldr-chip`, `.hero-btns`, `.hero-art-stage > .hero-art-card` + `.float-badge.{top-left,bottom-left,bottom-right}` | |
| Buttons | `.btn.btn-primary`, `.btn.btn-secondary`, `.small-btn`, `.reset-btn`, `.copy-rule-btn`; inside `.pledge`: `.btn`, `.btn-outline-light`, `.btn-emoji-pill` | |
| Ticker | `.ticker > .ticker-inner > span + span[aria-hidden]` | The content must be duplicated for the seamless `-50%` loop |
| Section header | `.section-header-block`, `.eyebrow`, `.section-title` | |
| Bento | `.bento-grid` → `.bento-card.bento-main`, `.bento-card.stat-card` (`.big` `.caption` `.tiny`), `.bento-subgrid > .mini-bento` (`.mini-bento-icon`), `.pillar-pills > .pillar-pill`, `.vibe-tag` | |
| Comparison | `.vibe-columns > .vibe-box.{green-flags,red-flags}` → `.vibe-head`, `.chat-list > .chat-bubble` (`.sender`, `.chat-verdict`) | WhatsApp-style bubbles |
| Filterable cards | `.rule-toolbar` (`.filter-box > .filter[data-filter]` + `.filter-count`, `.search-wrap > .search + .search-kbd`), `.rules-grid > article.rule[data-category][data-tone]` (`.rule-top` `.rule-number` `.rule-emoji-wrap` `.rule-footer` `.category`), `.empty` | `data-tone`: yellow / pink / green / blue. Copy the filter/search JS from `index.html` |
| Callout | `aside.media-note > .ico` | |
| CTA panel | `.pledge-zone > .pledge` (`.label`, `h2`, `p`, `.pledge-actions`, `.hint`) | Deep-teal block, the page's closing moment |
| Footer | `footer > .wrap.footer-row` (`.bengali`, `a.back`) | |
| Toast | `#toast.toast` + `.show`; the starter exposes `window.showToast(msg)` | |

## Behavior conventions
- Theme: `data-theme="light|dark"` on `<html>`, saved to `localStorage` key **`ba_theme`**. Pages share this key so the choice carries across the site. The toggle shows 🌙 in light mode and ☀️ in dark mode.
- Copy-to-clipboard: use `navigator.clipboard.writeText`, then `showToast('📋 … Ready to paste in WhatsApp.')`, and still show a toast in the `catch` branch. Shared WhatsApp text uses `*bold*` markdown and the prefix `📌 *Bangali Adda Lucknow* —`.
- Keyboard: `/` focuses the search box when there is one.
- Accessibility: `aria-labelledby` on sections, `aria-pressed` on toggle buttons, `role="status" aria-live="polite"` for live messages, and `aria-hidden` on decoration. `prefers-reduced-motion` already disables all animation globally; don't override it.

## Consistency checklist
- [ ] No `{{` placeholders remain.
- [ ] No raw hex colors in new component CSS (only `var(--…)`, apart from the fixed exceptions listed above). Every new token exists in both themes.
- [ ] Every heading uses Space Grotesk, and each h1/h2 has at most one `.serif-italic` accent.
- [ ] Raised surfaces use the 2px `--line-strong` border, a hard `--shadow-brutal`, and a token radius.
- [ ] Bengali text has `lang="bn"`. Decorative emoji and SVGs are `aria-hidden`.
- [ ] Checked in both light and dark themes, and at widths around 1280, 900, and 375px (no horizontal scroll and nothing overlapping the nav).
- [ ] The page is still a single self-contained HTML file with the shared `ba_theme` theme script.
