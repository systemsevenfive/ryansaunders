# Website Restructure — Design Spec
**Date:** 2026-05-04
**Status:** Approved

## Summary

Substantial update to ryansaunders.ca introducing a new Tools section, a Neon Arcade umbrella page, a full Nexus project page, and moving Sonny from Lab to Tools with updated content.

---

## 1. Homepage — Section Order and Navigation

### Section order (before → after)
| # | Before | After |
|---|--------|-------|
| 01 | Shipped | Shipped (unchanged) |
| 02 | In the Lab | Tools (new) |
| 03 | About | In the Lab (renumbered) |
| 04 | Contact | About (renumbered) |
| 05 | — | Contact (renumbered) |

### Navigation
Before: `Work · Lab · About · Contact`
After: `Work · Tools · Lab · About · Contact`

- `Tools` links to `#tools`
- `Work` continues to link to `#shipped`
- `Lab` continues to link to `#lab`

### Hero meta counters
Before: `3 Shipped · 5 In the lab · 42 On paper · Canada / Remote`
After: `3 Shipped · 2 Tools · 5 In the lab · 42 On paper · Canada / Remote`

The Tools chip uses `--accent-violet` (`#a78bfa`) to distinguish it from Shipped and Lab.

---

## 2. New Section: Tools

### Section header
- Number: `02`
- Title: `Tools`
- Section note: `Built & running`

### Grid layout
Two-column grid (matches Shipped's three-column grid pattern, adapted for two tiles).

### Tile design — `.tool-tile`
- Solid border (`--rule-strong`), same signal as Shipped (done work)
- Violet top-edge accent bar (2px gradient, `--accent-violet` → transparent)
- Monogram icon in place of app icon swatch (38×38px, violet background tint)
- `● Running` status in violet
- Hover: border-color → `--accent-violet`, `translateY(-2px)`

### Tiles

**Nexus**
- Monogram: `N`
- Headline: Nexus
- Body: "My personal dev workflow hub. Built because the alternatives were either too much or too little."
- Module tags (below body): `Tickets · Reports · Designs · Architecture · Glossary`
- Status: `● Running`
- Platform: `Local · Node + Web`
- Link: `/nexus/`

**Sonny**
- Monogram: `S`
- Headline: Sonny
- Body: "Analyzes a Swift codebase and maps its architecture as a navigable graph — types, dependencies, and relationships at a glance."
- Integration note (below body, monospace/faint): "Also available as a standalone package — integrated into Nexus for cross-project views."
- Status: `● Running`
- Platform: `Swift · npm package`
- Link: `/sonny/`

---

## 3. Updated Section: In the Lab

### Changes
- Renumbered from `02` to `03`
- **Sonny removed** (moves to Tools)
- **Neon Arcade tile added**
- Net tile count: 4 existing + 1 Neon Arcade = 5 tiles (unchanged visual count)

### Neon Arcade tile
Special variant of `.lab-tile` — uses accent pink rather than cyan to signal it's a different kind of project:
- Label: `ARCADE · GAMES`
- Title: `The Neon Arcade`
- Body: "Classics rebuilt for iPad. No IAP, no timers, no dark patterns. Just the game."
- Status: `IN DEVELOPMENT`
- Border: pink/accent tint (`rgba(255,45,149,0.35)`)
- Background: subtle pink tint (`rgba(255,45,149,0.03)`)
- Link: `/neon-arcade/`

### Next-nav chain repair
Sonny (EXP-005) moving out of Lab breaks the circular Lab next-nav. The chain should become:
`Ed Assists → ShopRoute → LCARS → Chartula → [wrap to Ed Assists]`
- `Chartula` next-nav: remove Sonny, wrap back to Ed Assists
- `Ed Assists` prev-nav: remove Sonny, wrap back to Chartula

---

## 4. New Page: `/nexus/`

### Structure
Follows the existing project page pattern with one addition: a **module strip** between the header and the prose.

### Title block
- Eyebrow: `Tools · Built & Running`
- H1: `Nexus`
- Tag: "My personal dev workflow hub. Tickets, reports, designs, architecture, and knowledge base — one interface, built to fit how I actually work."
- Meta grid: Status (`● Running` in violet), Year (`2025 — 2026`), Platform (`Node.js · Web`), Repo (`Local · Private`)
- Breadcrumb: `← Back to Tools` (links to `/#tools`)

### Module strip
Five equal-width cards between the header and the prose:
| Module | Description |
|--------|-------------|
| Tickets | Lightweight board and ticket system across all projects |
| Reports | Themed viewer for Claude session outputs — standups, post-mortems, insights |
| Designs | Living record of design decisions and Claude-authored mockups |
| Architecture | Sonny-powered graph views of every app project |
| Glossary | A navigable knowledge base of concepts and relationships |

### Hero image
`<img>` screenshot of the Nexus Reports hub (Aqua theme), captured with Option+Command-Shift-5 (no drop shadow). Placed between the module strip and the prose, same slot as `.hero-image` on other pages.

### Prose sections
1. **Why it exists** — Jira was too much, a text file wasn't enough. What the gap was. System grew from a ticket board as each missing piece became obvious.
2. **What I built** — The five systems described as an integrated whole, not a feature list. Tickets → auto-commit data repo. Reports → first-class themes. Designs → design record. Architecture → Sonny integration. Glossary → navigable graph.
3. **What's next** — Evolves with workflow; each gap becomes a ticket.

### Callout
"The best tool is the one that disappears. Nexus isn't interesting — my projects are. It just tries to stay out of the way."

### Tech stack pills
`Node.js · Fastify · Vanilla JS · Sonny · JSON · Git`

### Next-nav
Nexus and Sonny are the only two Tools pages. Next-nav wraps between them:
- Nexus: `← Sonny | Sonny →` (or just one direction if only two tiles)

---

## 5. Updated Page: `/sonny/`

### Changes
- Eyebrow: `Lab · Tinkering` → `Tools · Running`
- Breadcrumb: `← Back to Lab` → `← Back to Tools`
- Tag: update to reflect architecture-analysis focus
- Meta grid: Status → `● Running`, Track → remove `EXP-005 · AI`
- Prose content: rewrite to reflect the current tool — Swift codebase analysis, navigable architecture graphs, standalone npm package, Nexus integration
- Next-nav: remove Lab chain links, replace with Tools chain (Nexus ↔ Sonny)

---

## 6. New Page: `/neon-arcade/`

### Structure
Umbrella page — introduces the brand, then lists individual games as cards.

### Title block
- Eyebrow: `Lab · In Development`
- H1: `The Neon Arcade`
- Tag: "Classics rebuilt for iPad. No IAP, no energy timers, no dark patterns. Just the game."
- Meta grid: Status (`● In Development`), Platform (`iOS 26 · iPadOS 26`), Engine (`SpriteKit`), Titles (`2 in progress`)
- Breadcrumb: `← Back to Lab`

### Intro prose
Two paragraphs establishing the Ambrosia Software / shareware era framing. Key beat: that clarity of "here's the game, get better at it" that modern mobile has abandoned.

### Game cards — Option A (compact cards)
Stacked vertical cards, each containing:
- Inspiration label (monospace, cyan): e.g. `Inspired by Centipede / Apeiron`
- Game name (large, bold)
- Description paragraph
- Status + platform (right-aligned, monospace, faint)

**Scolonoids**
- Inspiration: `Inspired by Centipede / Apeiron`
- Description: Neon biomechanical Centipede. Gardener drone pruning a derelict living hive. Roguelike runs, charge beam, offset-drag touch input.
- Status: `IN DEVELOPMENT`

**Circle Back And Destroy**
- Inspiration: `Inspired by Harry the Handsome Executive`
- Description: Top-down multi-floor escape game. Navigate hostile office environments, hijack ally robots, and get out. Native Apple frameworks only — no Unity.
- Status: `EARLY DEV`

### Tech stack pills
`SpriteKit · SwiftUI · GameplayKit · GameController · Swift 6`

### Next-nav
Single Lab page, links back to other Lab items at the discretion of implementation (or omit next-nav since it's a different structure).

---

## 7. Files Affected

| File | Change |
|------|--------|
| `index.html` | New Tools section, reorder, new nav link, updated hero counters, Neon Arcade tile in Lab |
| `styles.css` | `.tool-tile` new class, `.lab-tile.neon-arcade` variant, section renumbering |
| `nexus/index.html` | New file |
| `neon-arcade/index.html` | New file |
| `sonny/index.html` | Content rewrite, eyebrow, breadcrumb, next-nav |
| `edassists/index.html` | Fix prev-nav (remove Sonny) |
| `chartula/index.html` | Fix next-nav (remove Sonny, wrap to Ed Assists) |
| `about/index.html` | Section renumber if hardcoded |
| `kitchentime/index.html` | Section renumber if applicable |

---

## 8. Design References (Nexus Designs)

- `2026-05-04-homepage-restructure-v1` — Overall structure and section order
- `2026-05-04-homepage-restructure-v2-updated-copy` — Tools tile copy and module approach
- `2026-05-04-nexus-page-structure-draft` — Nexus page layout and module strip
- `2026-05-04-neon-arcade-page-layout-options` — Neon Arcade game cards (Option A selected)
