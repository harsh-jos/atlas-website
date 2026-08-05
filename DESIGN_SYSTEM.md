# /atlas — Design System

The design system behind **slash-atlas.live**. This document exists so the site can be extended, rebuilt, or re-skinned by any agent or designer without guessing. It is a record of what is implemented in `index.html`, not a proposal.

> **Concept in one line:** knowledge is a map, not a list. Atlas is a learning system — you decide *what* to learn, Atlas handles *how*.
>
> **Visual identity:** warm paper, ink, deep map blue, light sky accent. One clean grotesque (General Sans), mono only for annotation labels. Zero serif. Light mode only.

---

## 1. Design tokens

All tokens live in `:root` inside `index.html`. Single source of truth.

### Color

| Token | Hex / value | Role |
|---|---|---|
| `--paper` | `#F6F2E9` | Page background. Warm ivory. |
| `--surface` | `#FCFAF4` | Card / panel background. One step lighter than paper. |
| `--ink` | `#201C16` | Primary text, primary buttons. |
| `--ink-2` | `#6E675C` | Secondary text (descriptions, annotations). |
| `--ink-3` | `#A39B8C` | Tertiary text (fineprint, indices, coordinates-era labels). |
| `--line` | `rgba(32,28,22,0.10)` | Hairlines, borders, grid. |
| `--line-strong` | `rgba(32,28,22,0.20)` | Stronger rules (underlines, section rules). |
| `--blue` | `#2E4A85` | *Interactive* blue: links, CTAs, waypoints, hub. The primary brand color. |
| `--blue-deep` | `#1F3566` | Hover state of `--blue`. |
| `--soft-blue` | `#E7ECF5` | Illustration panel background (behind cards' inner paper panels). |
| `--accent` | `#66A3FF` | *Highlight* accent: italic "Learn.", compass stars, destination star, signal dot. Light blue, use sparingly — it is decorative, not interactive. |

**Rules:**
- Blue = do something (links, buttons, waypoints). Accent = emphasize something (highlights, markers).
- Never put text on `--accent` at small sizes on paper (low contrast by design; it is a highlight, not an ink).
- Ink = buttons, paper = text on them. Buttons are ink → hover `--blue`.

### Typography

| Token | Font | Use |
|---|---|---|
| `--font-display` | **General Sans** (Fontshare, 400/500/600) | All headings and body copy. The whole voice. |
| `--font-sans` | General Sans | Alias for body/UI. |
| `--font-mono` | **Geist Mono** (Google Fonts, 400/500) | Annotation layer only: badges, indices, eyebrows, coordinates-style labels, morph, fineprint. |
| **Geist** (Google Fonts, 300/500) | Logo wordmark only — the `/` is 300, `atlas` is 500. Loaded exclusively for the logo lockup; nothing else uses it. |

No serif in the system. Italics are avoided — emphasis is done with weight and color.

### Spacing, radii, motion

| Token | Value | Use |
|---|---|---|
| `--container` | `1180px` | Max content width. Padded 32px desktop / 24px mobile. |
| Card radius | `22px` | Cards. |
| Pill radius | `999px` | Buttons, nav CTA. |
| Card height | `440px` (auto on mobile) | Fixed card body. |
| `--ease-out` | `cubic-bezier(0.16, 1, 0.3, 1)` | All transitions. |
| `--dur` | `250ms` | Hover/state transitions. |

---

## 2. Type scale (implemented)

| Role | Font | Size | Weight | Tracking | Notes |
|---|---|---|---|---|---|
| Hero headline | General Sans | `clamp(46px, 7.2vw, 92px)` | 500 | `-0.03em` | Line-height 1.04. Accent word in `--accent`. |
| Section headline (connect) | General Sans | `clamp(30px, 4.2vw, 46px)` | 500 | `-0.03em` | |
| Philosophy statement | General Sans | `clamp(30px, 4.4vw, 48px)` | 500 | `-0.025em` | |
| Card title | General Sans | `26px` | 500 | `-0.02em` | |
| Nav logo `/atlas` | Geist lockup | `23px` | 300 `/` + 500 `atlas` | `-0.6px` | Constellation mark beside it; see §7. |
| Transition line | General Sans | `22px` | 500 | `-0.01em` | |
| Hero morph | Geist Mono | `22px` | 400 | `0.01em` | The typed `/atlas`. |
| Hero annotation (right) | General Sans | `19px` | 500 | `-0.01em` | "Knowledge is a map, not a list." |
| Punch lines (hero) | General Sans | `19px` | 500 | `-0.01em` | "AI made information abundant." |
| Body / description | General Sans | `16px` | 400 | — | Ink-2. |
| Mono labels | Geist Mono | `10–11px` | 400/500 | `0.06–0.14em` | Uppercase, letterspaced. |

---

## 3. Layout & rhythm

- **Nav:** sticky, 72px, transparent → gains `rgba(246,242,233,0.86)` + `backdrop-filter: blur(14px)` + hairline after 8px scroll.
- **Hero:** `calc(100svh - 72px)`, centered column. Order: morph → headline → description → scroll indicator.
  - **Off-axis annotations** (desktop ≥1020px only): left = "YOU ARE HERE" (mono 11px, `0.14em`, uppercase, map-stamp convention); right = "Knowledge is a map, not a list." Both vertically centered against the hero.
- **Transition:** ~110px top / 100px bottom; hairline — text — hairline with a small accent dot at the right rule's end. One sentence: "Atlas is being built around three ideas."
- **Cards:** 3-up flex, 28px gap; `flex: 1 1 calc(50% - 14px)` at ≤900px; stacked at ≤720px.
- **Connect:** centered, eyebrow → h2 → copy → pill button → fineprint.
- **Philosophy:** big statement, rotating compass star ✦ (24s), mono line below.
- **Footer:** hairline top, `--paper`; logo left; tag + domain right. Text only — no links.

Vertical rhythm: sections are separated generously (110–170px). The page breathes; never crowd sections.

---

## 4. Components

### Nav
- Left: the full logo lockup — constellation mark + `/atlas` wordmark (Geist 300 `/`, 500 `atlas`; source `logo.svg`). Right: mono uppercase `GitHub` link + ink pill CTA ("Let's connect" → `#connect`).
- Mobile: wordmark + hamburger (links hidden).

### Hero morph (the brand moment)
- Terminal-style type-delete sequence, runs once, never loops:
  `slash-atlas.live` → (delete `.live`) → `slash-atlas` → (delete `slash-`, prepend `/`) → **`/atlas`**
- Timings: steps fire at 550ms / 1150ms / 1550ms; delete speed 40ms per char.
- On settle: the text transitions to `--blue` (`.settled` class) and the block cursor starts blinking — a terminal that found its brand.

### Cards
- Anatomy: index (`01`–`03`, mono, top-right, ink-3) · illustration panel (`--soft-blue`, 190px, contains an inner paper panel) · badge (dot + mono uppercase label) · title · description · CTA.
- Badge dots: `--blue` = available · `--accent` = coming soon (warn state).
- States: idle → hover = `translateY(-5px)`, border → `rgba(46,74,133,0.45)`, warm shadow `0 28px 48px -28px rgba(32,28,22,0.18)`, illustration scales 1.04, CTA arrow nudges.
- Disabled CTA ("Coming soon"): ink-3, no arrow, no hover.

### Illustrations (inline SVG, 260×152 on a 240×136 panel)
1. **Vault graph** (Research Agent): two aligned rows of note cards, hub note in `--blue`, pulse ring 3s, caption `obsidian://vault`. The signal color is `--accent`.
2. **Route** (Platform): dashed route drawn *through* numbered waypoints (start → 01–03 → destination star in `--accent`), faint alternative route behind, caption `slash-atlas.live/platform`.
3. **Skill file** (Skill): paper file with folded corner, window dots, YAML-ish keys (`learn:` `context:` `sources:` `memory:`), download chip with centered arrow.

**Alignment rule for future illustrations:** every waypoint/node must sit exactly on its path — make waypoints path endpoints, never free-floating coordinates. Rows of notes must share exact y-values.

### Connect section
- Eyebrow mono ("Say hello") → headline → one personal copy line → pill button (ink → hover blue) with LinkedIn glyph → mono fineprint.
- Button is an `<a>` (external, `rel="noopener"`), not a form. No fake states.

---

## 5. Motion (five animations, that's all)

| # | Animation | Spec |
|---|---|---|
| 1 | Hero morph | Type-delete sequence above; settles blue; cursor blinks 1s step-end. |
| 2 | Graph signal | A small accent dot travels edges via `animateMotion`, 8s loop. |
| 3 | Card hover | 250ms ease-out: lift, border, shadow, illustration scale. |
| 4 | Scroll reveal | `opacity 0 → 1` + `translateY(18px)`, 800ms ease-out, IntersectionObserver at 15% visibility, fires once. |
| 5 | Cursor blink | 1s `step-end` infinite. |

Ambient: hub pulse ring 3s, compass star rotation 24s, scroll indicator fade-pulse 3.2s.

All motion is killed by `prefers-reduced-motion` (durations forced to 0.01ms; morph jumps straight to `/atlas`).

---

## 6. Background & texture (layered, all vector)

Order back to front: paper → survey grid → grain → bloom → contours/graph fragments → content.

| Layer | Spec |
|---|---|
| Survey grid | Fixed, 64px hairlines at `--line`, opacity 0.5 of the var, radial mask fading toward edges. Reads as "map paper". |
| Grain | Fixed SVG `feTurbulence`, opacity 0.03, `multiply`. Adds paper tactility. |
| Bloom | Radial `--blue` at 0.09 → transparent, centered top. |
| Contours | Topographic hairlines, opacity 0.06, bottom corners of the hero. |
| Graph fragments | Sparse blue node-edge graphs with one mono label each ("vault", "map"), including the animated signal. |

---

## 7. Iconography & marks

- **Compass star ✦** (`&#10022;`): the recurring brand mark — hero annotation, philosophy, waitlist-era success. Rendered in `--accent`.
- **Constellation mark:** three `--accent` nodes joined by lines — the "knowledge graph" triangle that is the logo's mark. Used in the favicon, nav and footer. Not an emoji — it is the designed mark.
- **Logo files:** `logo.svg` (full lockup — mark + wordmark), `logo-mark.svg` (mark only, transparent, 44×44), `logo-wordmark.svg` (wordmark only). Wordmark = Geist 300 `/` + Geist 500 `atlas` on the original 340×88 canvas.
- **Favicon:** rounded square in `--blue` with the constellation mark (three `--accent` nodes, connecting lines at 0.8 opacity). Encoded as inline SVG data URI. The full logo (mark + wordmark) lives in `logo.svg`.
- **Arrows:** 13px stroke-1.3 line arrows; slide on hover. Download arrow on the skill card.
- **LinkedIn glyph:** standard logo path, `currentColor`, 16px, inside the connect pill.

---

## 8. Voice & copy

The map metaphor is the whole voice. Fixed lines:

- "Learning how to **Learn.**" (accent on the second word) — hero
- "AI made information abundant. Learning is still scarce." — hero punch
- "Knowledge is a map, not a list." — right annotation
- "YOU ARE HERE" — left annotation
- "Atlas is being built around three ideas." — transition
- "Got something to say? Let's connect." — connect headline
- "You decide what to learn. Atlas handles how." — philosophy
- "learning, mapped." — philosophy footer line
- "Built with curiosity." — site footer

Tone: first person ("I'm Harsh — the one building Atlas."), calm, no hype, no exclamation marks.

---

## 9. Accessibility

- Full `prefers-reduced-motion` support.
- `:focus-visible` = 2px `--blue` outline, 3px offset.
- `::selection` = blue background, paper text.
- All decorative SVGs are `aria-hidden`; the morph has an `aria-label` of its final state.
- Buttons are real `<a>` elements; the connect button opens in a new tab with `rel="noopener"`.
- Known trade-off: `--accent` on paper is low-contrast by design (highlight only). If it needs to carry text, darken toward `#4D8DF5`.

---

## 10. Implementation notes

- Single self-contained `index.html` — no build step. All CSS in one `<style>`, all JS in one `<script>`.
- Fonts: General Sans via Fontshare CDN; Geist (logo only) + Geist Mono via Google Fonts. All have system fallbacks.
- Favicon and OG tags are inline / in `<head>`; `og:image` expects an `og.png` in the site root (not yet added).
- Real URLs: obsidian-agent → `github.com/slash-atlas/obsidian-agent`; skill → `github.com/slash-atlas/atlas-skill`; general GitHub → `github.com/slash-atlas` (the org page aggregates all Atlas repos); connect → `linkedin.com/in/harsh-jos`.
- To re-skin: change the tokens in `:root`. The system is token-driven; nothing is hardcoded except inline SVG fills (noted in the illustration section).
