# Text-Minimal Rework — Design Spec
**Date:** 2026-05-07
**Status:** Approved

## Summary

Rework ryansaunders.com to be text-minimal. Sections 1–3 (Shipped, Tools, Lab) are untouched — Ryan handles those. The work covers: removing the About teaser on the homepage, replacing it with a compact AI workflow callout that links to the About page, and trimming the About page itself.

---

## 1. Scope

| Area | Change |
|------|--------|
| `index.html` — About teaser | Replace all content inside `.about-teaser` (the `<p>` and the `<a class="read-more">`) with the `.ai-callout` block |
| `index.html` — About section | Add `.ai-callout` block with mini pipeline + link |
| `about/index.html` — "How I work" | Remove section 02 entirely |
| `about/index.html` — "What I do" | Trim to one sentence |
| `about/index.html` — "How AI fits in" | Renumber to 02, trim prose to one paragraph, add full flow diagram below |
| `styles.css` | New styles: `.ai-callout`, `.mini-flow`, `.mini-node`, `.mini-arrow`, `.mini-gate-badge`, `.flow-wrap`, `.flow-node`, `.flow-node.gate`, `.flow-node.placeholder-node`, `.flow-eyebrow`, `.gate-tag` |

Sections 01 Shipped, 02 Tools, 03 Lab on the homepage are not touched.

---

## 2. Homepage — About Section

### Before

```html
<section class="about-teaser">
  <p>As a lifelong tecchie, hacker, tinkerer, and general enthusiast for all things computer-related, I've never been more excited about the possibilities.</p>
  <a class="read-more" href="/about/">Read more →</a>
</section>
```

### After

The `<p>` teaser is removed. Replace with a `.ai-callout` block:

```
[violet top-edge accent bar]
Mini pipeline row:
  Nexus / Tickets → Claude / Agents → Xcode · Git / [GATE badge] → Review / Every diff → Local LLMs / In progress (faded)
---
"iOS developer. Multi-agent workflow, human-reviewed."    "About me →"
```

**Callout styling:**
- Border: `--accent-violet-border`
- Background: `--accent-violet-soft`
- Top edge: 2px gradient `--accent-violet → transparent`
- Border-radius: `--r-lg` (14px)

**Mini pipeline:**
- Each node: tool name (regular weight, 12px) + role label (monospace, 9px, faint) stacked
- Arrows between nodes: `→` in `--accent-violet` at reduced opacity
- Gate node (Xcode · Git): role replaced by a cyan badge labelled `GATE`
- Local LLMs node: `opacity: 0.4` — placeholder signal

**Callout footer:**
- Left: `"iOS developer. Multi-agent workflow, human-reviewed."` — 13px, `--text-muted`
- Right: `"About me →"` — monospace, 11px, `--accent-violet`
- Separated from pipeline by a rule at `rgba(167,139,250,0.15)`

The `<a class="read-more" href="/about/">Read more →</a>` line is removed (the footer link replaces it).

---

## 3. About Page — /about/index.html

### Section changes

| Section | Before | After |
|---------|--------|-------|
| 01 What I do | Two sentences, one redundant | Two sentences trimmed: `"iOS developer — day job and day-off job. Most of my life spent in the Apple ecosystem, as a student, a creative, and a technical professional."` |
| 02 How I work | Present, one paragraph | **Removed entirely** |
| 03 How AI fits in | Three paragraphs, positioning language | Renumbered to 02. One paragraph + full flow diagram below |

### "How AI fits in" — renumbered to 02

**Prose (one paragraph):**
> Multi-agent workflow. Tickets live in Nexus. Claude agents pick them up and return pull requests. Build and tests must pass before a PR opens — no exceptions. I review every diff. The architecture decisions stay mine.

Ryan may refine this copy.

**Flow diagram** (immediately below the paragraph):

Five nodes in a horizontal connected strip:

| Node | Eyebrow | Title | Body |
|------|---------|-------|------|
| 1 | Nexus | Ticket | Scope and acceptance criteria defined before any code runs. |
| 2 | Claude | Agent | Picks up the ticket, writes the implementation, opens a draft PR. |
| 3 (gate) | Xcode · Git | Gate | Build passes. Tests pass. Formatting is clean. + `REQUIRED` badge |
| 4 | GitHub | Review | I read every diff. Architecture decisions stay mine. |
| 5 (placeholder) | Local LLMs | On-device | Private or latency-sensitive inference. In progress. |

**Flow styling:**
- Nodes share a connected border strip (adjacent borders merged via `border-left: none` on non-first nodes)
- First node: `border-radius: 10px 0 0 10px`; last node: `border-radius: 0 10px 10px 0`
- Arrow chevrons between nodes via `::after`/`::before` pseudo-elements
- Gate node (3): `rgba(0,200,255,0.04)` background, `rgba(0,200,255,0.2)` border, cyan eyebrow and badge
- Placeholder node (5): `border-style: dashed`, `opacity: 0.45`
- All eyebrows: `--accent-violet` (except gate = cyan, placeholder = faint)

---

## 4. CSS — New Classes

All new classes are additions; no existing classes are modified.

### `.ai-callout`
Callout container on the homepage. Violet-accented card, same border-radius as `.ship-tile`.

### `.mini-flow`
Flexbox row containing `.mini-node` and `.mini-arrow` elements.

### `.mini-node`
Stacked tool name + role label. `.mini-node.placeholder-node` at `opacity: 0.4`.

### `.mini-arrow`
The `→` glyph between nodes. `--accent-violet`, reduced opacity.

### `.mini-gate-badge`
Cyan pill replacing the role label on the gate node in the mini pipeline.

### `.callout-footer`
Flex row (space-between) for the summary sentence and "About me →" link.

### `.flow-wrap`
Flex container for the full flow on the About page. `align-items: stretch` so all nodes are equal height.

### `.flow-node`
Individual step. Adjacent left borders dropped via `+ .flow-node { border-left: none }`.

### `.flow-node.gate`
Cyan tint and border variant for the Xcode/Git gate step.

### `.flow-node.placeholder-node`
Dashed border, reduced opacity, for the Local LLMs step.

### `.flow-eyebrow`
Monospace label above the node title. Violet default, cyan for gate, faint for placeholder.

### `.gate-tag`
Cyan `REQUIRED` pill inside the gate node.

---

## 5. Design References (Nexus Designs)

- `2026-05-07-text-minimal-rework-v1` — Full before/after across all sections; open questions identified
- `2026-05-07-ai-section-layout-options` — Option A (blocks) vs Option B (flow) comparison
- `2026-05-07-ai-section-final-layout` — Approved final: homepage callout + About page full flow
