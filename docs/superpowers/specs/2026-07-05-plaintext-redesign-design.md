# Plaintext Redesign — Design Spec

**Date:** 2026-07-05
**Status:** Approved (design validated interactively in browser preview, 2026-07-05)
**Supersedes:** 2026-06-24 Quiet Tech redesign (visual layer only; structure and content decisions from that spec stand where not contradicted)

## Summary

Rebuild the site's visual language as relaxed web 1.0: monospace plaintext as the base layer — Courier, ASCII rules, bracket links, dotted-border restraint — contrasted by full-color neon project screenshots as the only saturated elements on the page. "Relaxed" means modern responsiveness is kept: sticky header, responsive card grids, elastic layout. Dark and light are both first-class, system-driven.

All copy is placeholder. Ryan rewrites every line himself in a later pass. Do not polish copy during implementation.

The approved reference implementation is the brainstorm mockup (scratchpad `index.html`, iterated live with Ryan on 2026-07-05). Its CSS is normative for the homepage and defines the site-wide token set.

## Design language

### Tokens

| Token | Dark (default) | Light |
|-------|----------------|-------|
| `--bg` | `#0a0a0a` | `#fafaf8` |
| `--text` | `#c8c8c8` | `#222` |
| `--text-dim` | `#7a7a7a` | `#6b6b6b` |
| `--rule` | `#444` | `#bbb` |
| `--img-border` | `#444` | `#999` |

Light mode via `@media (prefers-color-scheme: light)`. No manual toggle.

### Type

- Family: `"Courier New", Courier, monospace` — everything, headings included.
- Size: 15px base, 13px for captions/labels/badges. Headings stay 15px bold with `letter-spacing: 0.08em`; hierarchy comes from ASCII rules, not size.
- Line height 1.65. Headings and wordmarks in CAPS; body sentence case.

### Structural vocabulary

- **ASCII rules:** heavy `====` under the page h1, light `----` under each h2. Full container width, achieved by clipping a long glyph run (`overflow: hidden; white-space: nowrap`) — real characters, not CSS borders.
- **Bracket links:** nav and utility links render as `[link]`. Link hover inverts (text-on-bg → bg-on-text). All links underlined, text-colored (no link blue).
- **Cards:** 1px solid `--rule` border, transparent fill, no radius, no shadow. Footer row separated by 1px dashed rule. Border brightens to `--text-dim` on hover. Whole card clickable via stretched-link `::after` on the title anchor.
- **Status markers:** `● live` for shipped, `○ prototype/tinkering/in development` for lab.
- **Figures:** full-color screenshots with 1px border, dim 13px `fig N.` captions. Screenshots are the only saturated color on any page. Dark/light image variants via `<picture>` where assets exist.
- **Elastic ASCII arrows** (AI flow): dash runs that flex-fill available width, clipped, capped with `>`. Vertical stack with `↓` below 720px.
- **Inverted badge:** `background: var(--text); color: var(--bg)` for rare emphasis (e.g. the "gate" badge).

### Layout

- Page container `max-width: 1080px`, prose capped at ~72ch.
- Sticky top bar: wordmark left, `[work] [nexus] [lab] [about] [contact]` right, solid `--bg`, 1px dashed bottom rule, wraps to two lines on narrow screens. Sections get `scroll-margin-top` to clear it.
- Static page header below the bar: h1 wordmark + heavy rule + tagline lines. Both the sticky brand and the h1 stay (approved as-is).
- Grid breakpoints: cards 3-up ≥720px, stacked below. Nexus split (text left, screenshot right `5fr/7fr`) ≥900px.
- Smooth scrolling via CSS `scroll-behavior: smooth` only — no JS on the page.

## Homepage structure (approved)

1. Sticky top bar.
2. Header: h1 + `====` + taglines.
3. SHIPPED — three cards (icon, name, sub, `● live · version` + `[app store]` foot), then Kitchen Synk figure (fig 1).
4. NEXUS — split: entry text with dotted leader line and `built & running` status left, sessions screenshot right (fig 2).
5. IN THE LAB — three cards, no icons, dim label line (`exp-001 · tool`), `○ status` foot.
6. AI STUFF — recovered pre-trim pipeline `nexus → claude → xcode·git → review → local llms` with elastic arrows, gate badge inverted, `local llms` node dashed; dim one-line caption + about link below.
7. CONTACT — `[linkedin]`, Formspree form (mono inputs, 1px borders, inverting `send >` button).
8. Footer: light rule, © line, light-mode jab line.

## Scope

| Area | Change |
|------|--------|
| `styles.css` | Full rewrite to the plaintext system. All pages consume it. |
| `index.html` | Rebuilt to the approved reference markup. |
| `about/index.html` | Restyled to the same vocabulary; flow diagram uses the elastic-arrow treatment. |
| Project pages (`integenius`, `kitchensynk`, `solitairezero`, `nexus`, `edassists`, `cardinal`, `neon-arcade`, `sonny`, `lcars`, `shoproute`) | Same vocabulary: sticky bar, ASCII-ruled sections, bordered figures for screenshots. Structure preserved; markup adjusted only where the old design's components (gradient tiles, spotlight chrome) have no plaintext equivalent. |
| Privacy/contact boilerplate pages | Inherit restyled `styles.css`; no copy or structural changes. |
| Copy | Untouched placeholder throughout; Ryan rewrites all of it later. |

## Constraints

- This repo deploys the live site on push — no pushes; commits stay local until Ryan says otherwise.
- Visual changes previewed by Ryan in the browser before committing (established working agreement).
- No JS beyond what already exists per page; homepage drops its smooth-scroll script in favor of CSS.
- No web fonts, no icons fonts, no build step. Plain HTML + one CSS file, as today.
