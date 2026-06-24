# Quiet Tech Redesign — Design Spec
**Date:** 2026-06-24
**Status:** Approved
**Branch:** `feature/quiet-tech-redesign`

## Summary

Calm the portfolio's visual language. The current site runs the default "cyberpunk dashboard" look — a neon triad (pink/cyan/violet), a gradient-filled headline, glowing radial halos, glassy nav blur, and ALL-CAPS wide-tracked techno-labels — which is exactly why it reads as AI-generated.

The redesign — "Quiet tech" — keeps the site's spirit but turns the volume down: **one** desaturated magenta accent, system typography, restrained motion, sentence case, and less copy. Light and dark are both first-class. A single shared `styles.css` drives all 18 pages, so the stylesheet is the primary surface of change.

This intentionally calms artifacts introduced by the 2026-05-07 text-minimal rework (the homepage `.ai-callout` mini-flow and the About-page flow diagram), recoloring them off the violet/cyan accents.

---

## 1. Scope

| Area | Change |
|------|--------|
| `styles.css` | The bulk of the work. Rewrite the `:root` palette (and the `prefers-color-scheme: light` block) to one accent; remove gradients, halos, blur, and the pulse animation; repoint all cyan/violet component accents to neutral + sparing magenta; remove `text-transform: uppercase`; scope mono to captions; tune hero type and motion. |
| `index.html` | Trim the hero meta to one line; add a one-line tagline; simplify the "AI Stuff" mini-flow to a single quiet line. No structural rewrite. |
| `about/index.html` | Recolor/simplify the flow diagram to the neutral + magenta system; trim prose only if needed. |
| All other 16 pages | No direct edits — they inherit the calmed styles via `styles.css`. |

No page-local CSS or inline `<style>`/`style=` exists (verified), so class-level changes propagate everywhere automatically.

---

## 2. Palette

One accent only. The cyan and violet variables are **removed**; their consumers are repointed to neutral tokens, with magenta reserved for sparing use.

### Dark (default / signature)
| Token | Value |
|-------|-------|
| `--bg` | `#0C0C0D` |
| `--surface` | `rgba(255,255,255,0.02)` |
| `--text` | `#F2F2F0` |
| `--text-muted` | `rgba(255,255,255,0.58)` |
| `--text-faint` | `rgba(255,255,255,0.38)` |
| `--rule` | `rgba(255,255,255,0.11)` |
| `--rule-strong` | `rgba(255,255,255,0.16)` |
| `--accent` | `#D85C8E` |
| `--accent-soft` | `rgba(216,92,142,0.16)` |
| `--nav-bg` | `#0C0C0D` (solid, no blur) |
| `--tile-bg` | `transparent` (flat) |

### Light
| Token | Value |
|-------|-------|
| `--bg` | `#FAFAF8` |
| `--surface` | `rgba(0,0,0,0.015)` |
| `--text` | `#1A1A1A` |
| `--text-muted` | `rgba(0,0,0,0.56)` |
| `--text-faint` | `rgba(0,0,0,0.40)` |
| `--rule` | `rgba(0,0,0,0.11)` |
| `--rule-strong` | `rgba(0,0,0,0.16)` |
| `--accent` | `#AE2566` |
| `--accent-soft` | `rgba(174,37,102,0.10)` |
| `--nav-bg` | `#FAFAF8` (solid, no blur) |
| `--tile-bg` | `transparent` (flat) |

**Removed:** `--accent-cyan`, `--accent-cyan-soft`, `--accent-violet`, `--accent-violet-soft`, `--accent-violet-border`.

All values live in `:root` as the single source of truth — a future theme (e.g. a real neon opt-in) is a swap of this variable block, not a rewrite of component rules.

---

## 3. Removed — the AI tells

| Current | New |
|---------|-----|
| Gradient-clipped hero headline (`background-clip: text` → pink) | Solid `var(--text)` |
| `.hero-halo` radial glow gradients | Removed entirely |
| Gradient top-edge accent bars on `.spotlight-body`, `.tool-tile`, `.ai-callout` (`::before`) | Removed |
| Glassy nav (`backdrop-filter: blur(12px)`) | Solid `--nav-bg` + bottom hairline |
| Pulsing brand dot (`@keyframes pulse`, `box-shadow` glow) | Static magenta dot, no glow, no animation |
| ALL-CAPS + wide tracking on labels (`text-transform: uppercase`) | Sentence case, light tracking (~0.02–0.04em) |
| Three competing accents (pink + cyan + violet) | One magenta accent, used sparingly |
| Green/amber status colors (`#30d158` live, `#ff9f0a` soon) | Single magenta dot — the text label carries the meaning |

---

## 4. Kept — the calmed tech nod

- **Mono is reserved** for system-style captions only: `.sec-head .num` (section numerals), `.hero .eyebrow` + `.hero .meta`, the tile/case-study status foots, and the `.foot` ©/version line. Everywhere else uses the system sans.
- A single small **static** magenta dot in the brand; magenta status dots (`● Live`, `● Running`).
- Section numerals (`01`, `02`) stay — they read as quiet wayfinding, not decoration.

---

## 5. Typography

- System stack unchanged: `-apple-system, system-ui, sans-serif`; mono `SF Mono, Menlo, monospace`.
- Hero `h1`: weight `800 → 700`, letter-spacing `-0.045em → -0.02em`, size `clamp(46px,8vw,78px) → clamp(40px,6vw,60px)`.
- Body and prose sizes unchanged.
- Casing: remove `text-transform: uppercase` site-wide; labels go sentence case.

---

## 6. Motion

- Remove hover lift transforms (`translateY(-2px)`) and the hero halos.
- Remove the brand pulse animation.
- Keep subtle `border-color` / `color` transitions (~0.2s) for interactive surfaces.
- Hover on tiles: border to `--rule-strong` (neutral), no lift.
- Focus-visible: `2px solid var(--accent)` ring preserved.
- `prefers-reduced-motion` block preserved.

---

## 7. Components

Each is recolored to **neutral by default, magenta only where it signals state or interaction**.

| Component | Treatment |
|-----------|-----------|
| `.nav` | Solid `--nav-bg`, bottom hairline, no blur. Links sentence case, sans. Static magenta brand dot. |
| `.hero` | No halo, solid headline. Meta trimmed to one line; one tagline line added. |
| `.sec-head` | Mono numeral kept; label sentence case; note sentence case mono. |
| `.ship-tile` | Hairline border, flat bg, no gradient. Hover → `--rule-strong`. Status dot magenta. |
| `.spotlight-section` (Nexus) | Cyan border/top-bar/monogram/tags → neutral border + magenta link/status. Screenshot kept. |
| `.tool-tile` | Violet top-bar/monogram/tags → neutral + magenta accents. |
| `.lab-tile` | Solid hairline (drop dashed). `.lab-tile.neon-arcade` keeps **one** magenta pop (border + faint tint + label). |
| `.module-strip` / `.module-card` | Violet → neutral border, magenta name. |
| `.arcade-games` / `.arcade-game` | Cyan inspo label → magenta; meta sentence case. |
| `.ai-callout` / `.mini-flow` | Simplified to one quiet line on the homepage; violet → neutral. |
| `.flow-wrap` (About) | Recolor violet/cyan nodes to neutral; gate + placeholder kept but neutral/magenta. |
| `.btn` | `.primary` stays solid `--text`/`--bg`; `.ghost` neutral border → magenta on hover. |
| forms (`.contact-form`, `.form-group`) | Focus ring magenta; labels sentence case. |
| `.prose` / `.callout` / `.stack .pill` | Callout left-border + `em` highlight → magenta; pills neutral border. |
| `.next-nav` | Hover bg → `--accent-soft`; labels sentence case. |
| `.title-block` (case studies) | Solid headline; the live/running/soon status indicators collapse to the single magenta accent — the text label carries the meaning. |
| legacy `.privacy-content` / `.contact-content` | Links → magenta; inherits palette. |

---

## 8. Content trims

- **Hero meta:** drop the five-item stat row (`3 Shipped / 1 Tool / 5 In the lab / 42 On paper / Canada`) → one short line (e.g. `3 shipped · Canada / remote`) plus a single tagline.
- **AI Stuff (homepage):** simplify the mini pipeline to one quiet sentence + an "About →" link (the full five-node diagram can remain on the About page, recolored).
- **Voice stays:** the footer jab and playful sub-copy are personality, not stylization — keep them. Long-form writing is deferred to a future blog.

---

## 9. Themed pages

- **LCARS** (`/lcars/`): no custom CSS exists today; it adopts Quiet tech like any case study. Its Star Trek identity stays entirely in the copy (the "Tea, Earl Grey" callout, etc.).
- **Neon Arcade** (`/neon-arcade/`): adopts Quiet tech, with **one** deliberate magenta pop retained on its homepage `.lab-tile.neon-arcade`. The on-page games list recolors to neutral + magenta.

---

## 10. Accessibility

- Magenta link/text contrast meets WCAG AA for normal text in both modes (`#AE2566` on `#FAFAF8` ≈ 6:1; `#D85C8E` on `#0C0C0D` ≈ 5.4:1; AA needs 4.5:1). Verify exact ratios during implementation.
- `:focus-visible` rings preserved (magenta).
- `prefers-color-scheme` and `prefers-reduced-motion` preserved.
- No information conveyed by color alone (status dots are paired with text labels).

---

## 11. Deployment / Git

- ryansaunders.com is **live via GitHub Pages** (deploys on merge to `main`).
- Work happens on `feature/quiet-tech-redesign`, branched from an in-sync `main`.
- Per house rules for live sites: **no PR is opened, and nothing is pushed or merged, without Ryan's explicit approval.** Ryan merges; the agent does not.

---

## 12. Out of scope / future

- Genuine LCARS and arcade *skins* (net-new themed designs) — a separate additive project if ever wanted.
- A blog as the home for long-form copy.

---

## 13. Design references

- Inline Quiet-tech mockups produced this session (direction comparison + fuller homepage target, light + dark). Can be persisted to `~/Desktop/Dev/Nexus/designs/` for the Designs tab on request.
