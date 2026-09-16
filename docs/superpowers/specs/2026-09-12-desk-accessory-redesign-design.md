# Desk Accessory Redesign — Design Spec

**Date:** 2026-09-12
**Status:** Approved (design validated interactively via artifact mockup, 2026-09-12)
**Supersedes:** 2026-07-05 Plaintext redesign (visual layer only; content/structure decisions from that spec stand where not contradicted)
**Reference implementation:** homepage mockup artifact "Desk Accessory" (system-only theme build). Its CSS is normative for the homepage and defines the site-wide token set.

## Summary

Rebuild the site's visual language as **polished low-tech**: a real, fast website that borrows a couple of elements from Macintosh System 7.5 without wearing the OS as a costume. The reference is the era's restraint and craft, not its window chrome. Two specific 7.5 elements are folded in — a pinstripe title-bar motif shrunk to a masthead accent, and Finder list-view rows for the project lists — carried by quiet typography, hairline rules, and generous whitespace.

Explicitly moving away from: the current monospace-terminal "plaintext" look (which now reads as an AI-built trope), and the desktop/window metaphor explored during brainstorm (loses the sense of a website).

Explicitly avoiding common AI-built tells: gradients, purple/indigo accents, emoji, rounded-card grids, glassmorphism, centered hero, oversized display type.

All copy is placeholder. Ryan rewrites every line himself in a later pass. Do not polish copy during implementation.

## Design language

### Tokens

System-driven light/dark via `prefers-color-scheme`. No manual toggle. Three-state token pattern: light on bare `:root`; dark redefined under `@media (prefers-color-scheme: dark)` guarded `:root:not([data-theme="light"])`; dark also under `:root[data-theme="dark"]` (reserved — no UI sets it today, kept for a future opt-in).

| Token | Light | Dark |
|-------|-------|------|
| `--paper` (ground) | `#F4F2EA` | `#15151A` |
| `--ink` (text) | `#1B1A17` | `#E7E4DB` |
| `--dim` (muted) | `#6C6959` | `#938F80` |
| `--line` (hairline) | `#CBC6B8` | `#33333A` |
| `--line-2` (stronger edge) | `#B7B2A3` | `#43434C` |
| `--accent` (single accent) | `#3F5E86` | `#8FB0DC` |
| `--btn-hi` (bevel highlight) | `#FFFFFF` | `#33333A` |
| `--btn-sh` (bevel shadow) | `#CFCABA` | `#0D0D10` |
| `--sel` (`::selection`) | `#DDE6F0` | `#25303F` |

Warm off-white ground and warm near-black — chosen neutrals with a slight bias, not pure grey. A single muted Mac blue is the only accent; everything else is neutral.

### Type

No web fonts. Character comes from structure, not a downloaded typeface — keeps the page instant to load.

- `--sans` (names, body): `system-ui, -apple-system, "Segoe UI", Roboto, Helvetica, Arial, sans-serif`
- `--mono` (labels, status, nav, footer): `ui-monospace, "SF Mono", Menlo, Monaco, "Cascadia Mono", monospace`
- Base 16px, line-height 1.65. Section/utility labels are mono, uppercase, `letter-spacing: .16em`, ~11.5px. Page h1 `clamp(26px, 6vw, 34px)`, `letter-spacing: -.015em`, `text-wrap: balance`. Names 17px/600.

### Structural vocabulary

- **Pinstripe masthead (7.5 element #1):** a small ~22×15px block of `repeating-linear-gradient(--ink 0 1px, transparent 1px 3px)` sitting beside the wordmark in the sticky top bar — the one title-bar nod, flat and integrated, not a window.
- **Close-box wink:** a single 10px square outline (`1.5px solid --ink`, 2px radius) before the wordmark. A quiet reference, not functional.
- **Finder list-view rows (7.5 element #2):** Work and Lab render as list rows — `name · one-line blurb · status` — on a grid with hairline dividers top and between. Name is a link (600); blurb is `--dim`; status is mono. Status markers: `● live` (accent dot) for shipped, `○ prototype / tinkering / in dev` for lab.
- **Mono small-caps labels:** each section opens with a mono uppercase label whose trailing hairline rule fills the remaining width (`.lbl::after` flex rule).
- **Beveled outline button:** the single CTA (e.g. App Store) is an outline button with a 1px inset bevel (`inset 1px 1px 0 --btn-hi, inset -1px -1px 0 --btn-sh`) — the lone skeuomorphic touch.
- **Hairlines everywhere else:** 1px `--line` rules separate sections, list rows, top bar, and footer. No shadows (except the button bevel), no card fills, no radii beyond small control rounding.

### Layout & responsiveness

- Reading column `max-width: 720px`, centered; horizontal padding `clamp(18px, 5vw, 28px)`. On large monitors the column stays capped — no full-bleed sprawl.
- **Sticky top bar:** wordmark (close-box + pinstripe + name) left, mono nav (`work · lab · about · contact`) right via `margin-left:auto`, `flex-wrap` to a second line on narrow screens. 1px bottom hairline.
- **List rows:** desktop grid `minmax(0,1fr) minmax(0,1.5fr) auto` (name / blurb / status).
- **Phone breakpoint (≤560px):** grid collapses to `1fr auto` — name + status on the first line, blurb spans full width beneath (`grid-column: 1 / -1; order: 3`).
- Sizing is fluid across phone → tablet → desktop → large monitor with `clamp()` on padding and h1; no fixed device widths.

## Homepage structure (approved)

1. Sticky top bar: close-box + pinstripe + wordmark, mono nav.
2. Header: h1 wordmark, mono role line (`iOS developer · Canada / remote`), one intro paragraph (placeholder).
3. WORK — mono label + rule, three Finder list rows with `● live` status.
4. IN THE LAB — mono label + rule, three list rows with `○ status`.
5. CONTACT — mono label + rule, `linkedin · email` links, one beveled App Store button.
6. Footer: hairline, © line + a short right-aligned note.

## Scope

Build sequence: **homepage first.** This pass nails `styles.css` (the shared token system + all shared components) and rebuilds `index.html`. The other 17 pages inherit the restyled `styles.css` in a **second pass** (own spec/plan increment).

| Area | This pass |
|------|-----------|
| `styles.css` | Full rewrite to the Desk Accessory token system + shared vocabulary (top bar, labels, list rows, button, footer). |
| `index.html` | Rebuilt to the approved reference markup. |
| about / 10 project pages / privacy / contact boilerplate | **Deferred** to the second pass; they keep their (stripped) markup and will be restyled to the same vocabulary later. |
| Copy | Untouched placeholder throughout; Ryan rewrites all of it later. |

## Constraints

- **Target:** the blank-slate copy `ryansaunders-copy/`. It has no `.git` and **deploys nowhere** — safe to iterate freely. Not the live `ryansaunders/` repo.
- No web fonts, no icon fonts, no framework, no build step. Plain HTML + one CSS file.
- No images required in the page shell; the design holds with zero assets loaded.
- System-driven theme only — no toggle, no theme JS. Effectively **no JavaScript** on the homepage.
- Visual changes previewed by Ryan (artifact mockup, then the built page) before the second-pass rollout.
