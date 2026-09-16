# Desk Accessory — Inner Pages (Pass 2) Design Spec

**Date:** 2026-09-13
**Status:** Approved (design validated interactively via artifact mockup "Case Study Template", 2026-09-13)
**Builds on:** 2026-09-12 Desk Accessory Redesign (homepage). Same token set, type, and vocabulary — this spec extends them to the 17 inner pages.
**Reference implementation:** the "Case Study Template" mockup artifact. Its CSS is normative for the case-study, form, and privacy treatments.

## Summary

Extend the Desk Accessory visual language from the homepage to the remaining 17 pages of the copy, and unify the shared chrome (masthead + footer) so the site reads as one. No new tokens, no web fonts, no framework. The existing per-page content markup (title blocks, prose, figures, forms) is kept and restyled through `styles.css`; only the shared nav/footer markup is replaced.

## Page-type buckets

| Bucket | Count | Pages |
|--------|-------|-------|
| Case study | 10 | integenius, kitchensynk, solitairezero, nexus, edassists, cardinal, neon-arcade, sonny, lcars, shoproute |
| About | 1 | about |
| Privacy | 3 | integenius/privacy, kitchensynk/privacy, solitairezero/privacy |
| Contact | 3 | integenius/contact, kitchensynk/contact, solitairezero/contact |

## Component treatments

All carry the homepage vocabulary: warm paper/ink tokens, `--accent` as the only color, mono small-caps labels, hairline rules, the beveled button, system-driven light/dark.

- **Crumb** — mono, dim, `← back to work`.
- **Title block** — mono uppercase eyebrow; h1 at the homepage scale (`clamp(26px,6vw,34px)`), sentence case (drops the old uppercase + `====` rule); dim tag line ≤60ch.
- **Meta grid** — flex-wrap row; each cell a mono uppercase label over a bold sans value; `● live` uses `--accent`. Platform SVGs kept, inherit ink.
- **Hero image** — full-width with a 1px `--line-2` border; dim mono caption.
- **Before/after slider** (kitchensynk, nexus) — the existing `hero-reveal` JS is kept; chrome restyled to a 1px divider and a small beveled square handle.
- **Section headings** (`.prose h2`) — reuse the homepage mono label + trailing hairline rule; the old `.num` markers (`00/01/02`) are hidden (the sections are not a true sequence).
- **Prose** — sans body ≤65ch; unordered lists get a small square marker.
- **Feature list** — hairline-separated rows with a small square `--accent` marker.
- **Pills** (`.stack .pill`) — quiet mono outline tags.
- **Callout** — dashed-hairline aside, dim text.
- **Next-nav** — prev/next, mono uppercase labels over bold titles, hairline top; hover underlines the title in `--accent`.
- **Forms** (contact) — mono uppercase field labels; 1px inputs on paper; `--accent` focus ring; beveled submit.
- **Privacy / contact containers** — narrow reading column; h1 title block; mono-label sections; dim prose; mono last-updated + back-link.

### One-offs

- **Module strip** (nexus) — responsive grid of hairline cards: bold module name + dim description.
- **Arcade game cards** (neon-arcade) — hairline cells: mono inspiration line, bold title, dim description, hairline meta footer.
- **AI-flow diagram** (about) — `flow-node` boxes with 1px hairline borders and mono role labels, joined by thin `→` connectors; stacks with `↓` below 620px. Replaces the old elastic ASCII arrows. `gate-badge` inverts (ink ground, paper text).

## Shared chrome unification

Every inner page's `<nav class="nav">…</nav>` is replaced with the homepage masthead (`.top` → close-box + pinstripe + wordmark + mono nav), and its `<footer class="foot">…</footer>` with the homepage footer. Nav links are uniform: brand → `/`, `work` → `/#work`, `lab` → `/#lab`, `about` → `/about/`, `contact` → `/#contact`.

## Scope

| Area | This pass |
|------|-----------|
| `styles.css` | Append the inner-page component section (all classes above). Homepage section untouched. |
| 17 inner pages | Replace shared masthead + footer markup only. Content markup (title blocks, prose, figures, forms, one-offs) is kept and styled via CSS. |
| `index.html` | Untouched (done in Pass 1). |
| Copy | Placeholder throughout; Ryan rewrites later. |

## Constraints

- **Target:** `ryansaunders-copy/` — no `.git`, deploys nowhere.
- The `hero-reveal` slider JavaScript on kitchensynk and nexus is preserved as-is; only its CSS chrome changes.
- No web fonts, no icon fonts, no framework, no build step. One CSS file.
- System-driven theme only; no toggle. No new JavaScript.
- Every page must hold at all widths (phone → large monitor) with no horizontal overflow, in both themes.
