# DESIGN.md — Bangali Adda Lucknow

The visual and interaction language of the Bangali Adda site. `index.html` is the reference implementation. Values in this file are copied from it. If the two disagree, `index.html` wins, so update this file to match.

## 1. Concept

**"Tehzeeb meets Addabazi."** Bengal's warmth and Lucknow's courtly grace, presented as a playful, Gen-Z-friendly community zine.

- **Warm neo-brutalism:** cream paper, thick ink outlines, hard offset shadows, pill shapes, and springy hover lifts.
- **Cultural layering:** Bengali alpana and kantha motifs alongside Lucknawi chikankari jaali, Rumi Darwaza and the Howrah Bridge, all rendered as faint, theme-aware SVG backdrops.
- **Bilingual voice:** English first, seasoned with Bengali (and some Hindi/Urdu) words and emoji.

## 2. Color

All colors are CSS custom properties on `:root` (light), overridden by `[data-theme="dark"]` on `<html>`. Components only reference tokens.

| Token | Light | Dark | Role |
|---|---|---|---|
| `--bg-base` | `#faeed7` | `#130f0d` | Page background, inputs |
| `--bg-elevated` | `rgba(255,250,240,.84)` | `rgba(28,22,19,.84)` | Full-bleed bands |
| `--bg-card` | `rgba(255,252,246,.92)` | `rgba(31,25,21,.9)` | Translucent cards (paired with backdrop blur) |
| `--bg-card-solid` | `#fffdf8` | `#1e1814` | Opaque cards, pills |
| `--bg-glass` | `rgba(255,249,238,.8)` | `rgba(24,19,16,.8)` | Sticky nav |
| `--ink` | `#1e1915` | `#f9f3e8` | Headings, primary text |
| `--ink-soft` | `#3a312a` | `#e4d9c8` | Body copy |
| `--muted` | `#695d52` | `#a89b8c` | Captions, meta |
| `--line` | `rgba(30,25,21,.14)` | `rgba(249,243,232,.14)` | Dividers (often dashed) |
| `--line-strong` | `#1e1915` | `rgba(249,243,232,.78)` | Outlines, brutal shadows |
| `--sindoor` | `#e84e1b` | `#ff6b3d` | **Primary accent**: CTAs, serif accents, hovers, selection |
| `--sindoor-soft` | `rgba(232,78,27,.14)` | `rgba(255,107,61,.18)` | Hover fills, focus rings |
| `--turmeric` | `#f9bf3b` | `#ffd15c` | Highlight swash, dark-mode CTA, toast border |
| `--turmeric-soft` | `#fff0be` | `rgba(255,209,92,.15)` | Eyebrows, icon tiles |
| `--teal` | `#0f5e47` | `#2dd4a0` | Stats, positive states |
| `--teal-soft` | `#dcf2e9` | `rgba(45,212,160,.14)` | Green tones |
| `--royal-awadh` | `#8c2d4b` | `#e06d94` | Sparing heritage accent |
| `--rose` / `--rose-soft` | `#ff9eaa` / `#ffe8ec` | `#ff94a4` / `rgba(255,148,164,.14)` | Pink tones, red flags |
| `--sky-soft` | `#e3f2fd` | `rgba(96,176,255,.15)` | Blue tone |
| `--lavender-soft` | `#efe8ff` | `rgba(186,156,255,.15)` | Purple tone |

**Decorative tokens:** `--alpana-stroke`, `--chikan-stroke`, `--skyline-back|mid|front` and `--hero-mesh-1|2|3`. These drive the background art so it re-themes automatically.

**Fixed (theme-independent) colors:** the ticker and toast `#1e1915` with cream text `#fff8ec`; the pledge panel gradient `#10493a → #0a3328` with text `#fff7e9`; white on sindoor buttons; the live dot `#16a34a`; the "agreed" state `#9ef0b4`.

**Rules:** never hard-code a color in a component. Every new token gets a value in both themes. Sindoor is the only "loud" color, so use at most one sindoor element per visual group.

## 3. Typography

Loaded from Google Fonts in a single `<link>`.

| Token | Family | Weights | Use |
|---|---|---|---|
| `--font-display` | Space Grotesk | 500–700 (rendered at 800) | All headings, big numbers, brand |
| `--font-serif` | Instrument Serif *italic* | 400 | One accent word per heading, in sindoor |
| `--font-sans` | Plus Jakarta Sans | 400–800 | Body (line-height 1.6), buttons |
| `--font-mono` | Space Mono | 400, 700 | Eyebrows, tags, counters: UPPERCASE, tracked |
| `--font-bn` | Hind Siliguri | 500–700 | All Bengali script (`lang="bn"`) |

| Element | Size | Line-height | Tracking |
|---|---|---|---|
| Hero h1 | `clamp(54px, 7.6vw, 102px)` → mobile `clamp(40px, 11.5vw, 56px)` | .94 → 1.06 | `-0.055em` |
| Section h2 | `clamp(34px, 4.8vw, 56px)` → mobile `clamp(28px, 8vw, 38px)` | 1.08 | `-0.04em` |
| Card h3 | 22px | 1.18 | `-0.03em` |
| Mini card h4 | 19px | — | `-0.02em` |
| Lead | `clamp(16.5px, 1.8vw, 19.5px)` | 1.68 | — |
| Body / card text | 14.5–16.5px | 1.6 | — |
| Mono labels | 11–12px | — | `0.04–0.18em` |
| Stat number | `clamp(64px, 7.5vw, 100px)`, teal | .95 | `-0.06em` |

**Serif accent:** `<span class="serif-italic">word</span>` inside an h1 or h2, set at about 1.06–1.08em. In the hero it also gets a tilted turmeric underline swash (`::after`, rotated -1.5deg).

## 4. Shape, depth and spacing

| Token / value | Use |
|---|---|
| `--radius-xl` 28px | Bento cards, vibe boxes |
| `--radius-lg` 22px | Rule cards, mini cards, empty state |
| `--radius-md` 16px | Chat bubbles (`16 16 16 4`), float badges |
| 34px | Hero art card, pledge panel |
| 999px | Every button, pill, chip, nav, input |
| `--shadow-brutal` `4px 4px 0` | Default raised surface |
| `--shadow-brutal-lg` `6px 6px 0` | Hover state, hero art, pledge |
| `--shadow-sm` / `--shadow-md` | Soft ambient depth, layered under brutal shadows |
| `2px 2px 0 var(--line-strong)` | Small tiles, icon boxes, small buttons |

- **Borders:** 2px `--line-strong` on cards and buttons, 1.5px on pills, tags and inputs, 1px dashed `--line` for internal dividers.
- **Container:** `.wrap` = `min(1180px, calc(100% - 40px))`, or `100% - 28px` on mobile.
- **Section padding:** about `10px 0 84px`. Banded sections use `68px 0 92px`, and the pledge uses `88px 0 96px`.
- **Gaps:** 20–22px between cards, 8–14px within groups.

## 5. Motion

| Pattern | Spec |
|---|---|
| Springy easing | `cubic-bezier(0.34, 1.56, 0.64, 1)`, 0.2–0.28s |
| Card / button hover | `translate(-2px, -3…-5px)` plus the larger brutal shadow |
| Icon hover | brand icon `rotate(-8deg) scale(1.06)`, theme button `rotate(12deg)` |
| Float badges | `floatBob` 5s ease-in-out infinite, with staggered negative delays |
| Ticker | `slide` 32s linear infinite (duplicated content, `-50%`) |
| Alpana mandalas | `slowSpin` 95s and reverse 115s |
| Live dot | `pulseGreen` 2s |
| Theme switch | `background-color` and `color` transition 0.35s |
| Toast | slides up from `translateY(120%)`, auto-hides after 2.6s |

`prefers-reduced-motion: reduce` turns off all animation and transitions globally, and new work must respect it.

## 6. Background stack

Fixed, `aria-hidden` and `pointer-events: none`, from back to front:

1. `.bg-stage`: four radial gradients from the hero-mesh tokens over `--bg-base`.
2. `.bg-pattern`: a 160px SVG tile of a jaali diamond lattice and an alpana lotus (opacity .85, or .45 in dark mode).
3. `.lucknawi-canopy`: a scalloped mehrab/toran strip at the top, with marigold tassels.
4. `.cultural-float-layer`: giant rotated watermark words (আড্ডা, "Tehzeeb & Adda", ভালোবাসা • लखनऊ) at about 6% opacity.
5. `.alpana-watermark` ×2: slowly rotating mandalas in opposite corners.
6. `.cultural-horizon`: a 290px skyline of Rumi Darwaza and Imambara (left), the Howrah Bridge and a Bengal temple (right), and a river with a nauka in the middle.
7. `.grain`: a paper-noise overlay (z-index 40, opacity .11).

## 7. Components

| Component | Anatomy | Key styling |
|---|---|---|
| **Nav** | Sticky floating pill: brand (gradient sindoor icon tile + "bangali**adda.**" + Bengali tag), links, live status pill, theme button, small CTA | Glass blur, 68px tall, 2 layered shadows |
| **Eyebrow** | `✳ Short label` | Mono uppercase pill on turmeric-soft with a `3px` brutal shadow |
| **Primary button** | `.btn.btn-primary` | Sindoor with white text, 14×26px padding, 800 weight, trailing arrow glyph |
| **Secondary button** | `.btn.btn-secondary` | Solid card background with ink text |
| **Small button** | `.small-btn` | Sindoor pill, turning turmeric with dark text in dark mode |
| **Chips** | `.tldr-chip`, `.pillar-pill`, `.vibe-tag`, `.rule-number` | Emoji + short label |
| **Bento card** | Tag, h3, copy, pills | Translucent card with a sindoor radial glow in the corner |
| **Stat card** | Tag, huge teal number, caption | Teal-soft gradient |
| **Mini bento** | Tinted 44px emoji tile, h4, muted copy | Lifts 4px on hover |
| **Rule card** | Number pill + emoji tile / h3 / text / dashed footer (category + copy button) | `data-tone` yellow/pink/green/blue gives a 160deg tint gradient |
| **Toolbar** | Filter pills (active = ink fill) with mono counts, plus a search pill with a `/` key hint | Solid card with a brutal shadow |
| **Vibe box** | Header with h3 and tag, then a stack of chat bubbles | Green (teal) or red (rose) top-tint gradient |
| **Chat bubble** | Mono sindoor `@sender`, message text, verdict badge | WhatsApp tail radius `16 16 16 4` |
| **Callout** | `aside.media-note`: emoji tile plus a bold lead-in | |
| **Pledge / CTA panel** | Mono label, big h2, copy, turmeric button with a dark emoji pill, ghost button, live hint | Deep-teal block, giant faint ✳ in the corners |
| **Ticker** | Uppercase mono words separated by turmeric ✳ | Dark band, rotated -0.9deg |
| **Footer** | Name and motto · Bengali sign-off in sindoor · "Back to top ↑" pill | |
| **Toast** | Bottom-right dark pill with a turmeric border | |
| **Empty state** | Emoji, bold line, hint, turmeric reset button | Dashed border |

## 8. Responsive

| Breakpoint | Changes |
|---|---|
| ≤ 960px | Hide the nav links, status pill and Bengali brand tag. The hero, bento and vibe sections go to 1 column, and the rules grid to 2 |
| ≤ 650px | Smaller nav (60px) and type. Float badges become static inline chips. Hero and pledge buttons go full-width, filters become a 2×2 grid, and the rules grid goes to 1 column |

Add new mobile rules inside these two existing media blocks instead of creating new breakpoints.

## 9. Voice and content

- **Tone:** warm, inclusive, witty, never preachy. Gen-Z slang is welcome in small doses ("vibe check", "+100 Aura", "W Post").
- **Bilingual:** *Nomoshkar & Adaab*, *adda*, *para*, *cha*, *Shubho Adda*, *আড্ডা চলুক!*. Bengali always gets `lang="bn"` and the Bengali font.
- **Glyphs:** `✳` (eyebrows, ticker), `✦` (labels, categories), `·` (inline separators), `↗` (outbound/primary CTA), `↑` (back to top).
- **Emoji** act as icons. Decorative ones get `aria-hidden="true"`.
- **References** pair Bengal (Pujo, dhak, cha in a bhar, Howrah, Nolen Gur) with Lucknow (Rumi Darwaza, chikankari, tehzeeb, Hazratganj, Gomti Nagar).
- **Shared WhatsApp text** uses `*bold*` and the prefix `📌 *Bangali Adda Lucknow* —`.

## 10. Illustration

Original inline SVG only. Use thick `#29251f` outlines (3–6px), flat warm fills (`#ffc65c`, `#bf5b31`, `#e84e1b`, `#176c55`, `#f7a6a4`, `#f8f0d9`), dashed accent strokes, sparkle stars and cultural objects (bhar cup, dhak, alpana medallion, arches, bridge). Background art uses the decorative tokens so it themes itself. The wrapper gets `role="img"` and an `aria-label`.

## 11. Brand icon and social card

- **Favicon** (`assets/favicon.svg`): a 54px sindoor-gradient tile (`#e84e1b → #ff8547`) with a 3px ink border and a hard 4px ink offset shadow, holding a cream clay bhar with steam. It's the vector twin of `.brand-icon`. Don't use the ☕ emoji as an icon, because emoji render differently on every OS.
- **OG card** (`assets/og-image.html` → `og-image.jpg`, 1200×630): cream mesh background with the jaali pattern, an inset brutal frame (3px border, 34px radius, 8px shadow), brand lockup, eyebrow, the hero headline with its serif accent and swash, a one-line lead, and the hero illustration with a Bengali float badge. Page-specific cards should keep this layout and change only the text.

## 12. Interaction and accessibility

- **Theme:** `data-theme` on `<html>`, saved in `localStorage` under `ba_theme`, shared across pages. The toggle shows 🌙 in light mode and ☀️ in dark mode.
- **Toggle buttons** use `aria-pressed`. Live messages use `role="status" aria-live="polite"`.
- **Sections** use `aria-labelledby` pointing at their heading id.
- **Focus:** inputs show a sindoor border plus a 3px `--sindoor-soft` ring.
- **Keyboard:** `/` focuses search.
- **Selection** is sindoor with white text.
- **Anchor offset:** `scroll-padding-top: 96px` clears the sticky nav.
