# Quiet Tech Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Calm ryansaunders.com from a neon "cyberpunk dashboard" look to "Quiet tech" — one desaturated magenta accent, system type, restrained motion, sentence case, minimal copy — in both light and dark, by editing the single shared `styles.css` plus light HTML trims.

**Architecture:** Static site, vanilla HTML/CSS, deployed via GitHub Pages. One `styles.css` (1582 lines) drives all 18 pages through CSS classes and a `:root` variable palette with a `prefers-color-scheme: light` override. There is **no build step and no test runner** — see Verification Approach. Spec: `docs/superpowers/specs/2026-06-24-portfolio-quiet-tech-redesign-design.md`.

**Tech Stack:** HTML5, CSS3 (custom properties, `prefers-color-scheme`), GitHub Pages. Verification via `grep` and a local `python3 -m http.server` + browser screenshots.

---

## Verification Approach (read first)

This is a visual restyle, not application logic, so "tests" are:

1. **Static `grep` assertions** on `styles.css` — deterministic checks that a banned pattern is gone (e.g. no `backdrop-filter`, no `--accent-cyan`). Each task lists its grep checks.
2. **Visual checks** — serve locally and eyeball the affected pages in **both** color schemes.

**One-time setup (no commit):**

- [ ] Start a local server from the site root:

```bash
cd /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders
python3 -m http.server 8081
```

- [ ] Open `http://localhost:8081/` in a browser. Confirm the site loads.
- [ ] Capture baseline screenshots of the homepage in **dark** (default) and **light**. Emulate light via DevTools → Rendering → "Emulate CSS media feature prefers-color-scheme: light", or macOS System Settings → Appearance → Light. Keep these to compare against.

Keep the server running for the whole session. All "visual check" steps below use it.

**Ordering rule that prevents breakage:** the `--accent-cyan*` / `--accent-violet*` variables stay **defined** until Task 8. Every task that removes a *reference* to them runs before Task 8, so no intermediate state has a dangling `var()`.

---

## File Structure

| File | Responsibility | Change type |
|------|----------------|-------------|
| `styles.css` | Entire visual system for all 18 pages | Heavy edits (palette, de-neon, recolor, type, casing, motion, var cleanup) |
| `index.html` | Homepage | Light edits (hero meta + tagline, remove halo div, simplify AI callout) |
| `about/index.html` | About page | Verify only (flow diagram recolors via CSS); trim prose only if needed |

No new files. No page-local CSS exists, so class edits propagate to all pages automatically.

---

## Task 1: Palette variables

Update the neutral/text/accent tokens to Quiet-tech values in both `:root` blocks. **Leave the `--accent-cyan*` and `--accent-violet*` names defined for now** (removed in Task 8).

**Files:**
- Modify: `styles.css` — `:root` (dark, ~lines 5–40) and `@media (prefers-color-scheme: light) :root` (~lines 42–61)

- [ ] **Step 1: Define the checks**

After the edit:
- Dark homepage canvas reads near-black `#0C0C0D`; text warm-white.
- Light canvas reads warm off-white `#FAFAF8`.
- `grep -n -- '--tile-bg' styles.css` shows `transparent` (no gradient) in both blocks.

- [ ] **Step 2: Edit the dark `:root` palette**

Replace the dark palette declarations with:

```css
  --bg:            #0C0C0D;
  --surface:       rgba(255, 255, 255, 0.02);
  --text:          #F2F2F0;
  --text-muted:    rgba(255, 255, 255, 0.58);
  --text-faint:    rgba(255, 255, 255, 0.38);
  --rule:          rgba(255, 255, 255, 0.11);
  --rule-strong:   rgba(255, 255, 255, 0.16);
  --accent:        #D85C8E;
  --accent-soft:   rgba(216, 92, 142, 0.16);
  --nav-bg:        #0C0C0D;
  --tile-bg:       transparent;
```

Leave the existing `--accent-cyan`, `--accent-cyan-soft`, `--accent-violet`, `--accent-violet-soft`, `--accent-violet-border` lines untouched for now. Leave the type/radius/layout variables untouched.

- [ ] **Step 3: Edit the light `:root` palette**

Replace the light-mode declarations with:

```css
    --bg:          #FAFAF8;
    --surface:     rgba(0, 0, 0, 0.015);
    --text:        #1A1A1A;
    --text-muted:  rgba(0, 0, 0, 0.56);
    --text-faint:  rgba(0, 0, 0, 0.40);
    --rule:        rgba(0, 0, 0, 0.11);
    --rule-strong: rgba(0, 0, 0, 0.16);
    --accent:      #AE2566;
    --accent-soft: rgba(174, 37, 102, 0.10);
    --nav-bg:      #FAFAF8;
    --tile-bg:     transparent;
```

- [ ] **Step 4: Run the grep check**

```bash
grep -n -- '--tile-bg' styles.css
```
Expected: both occurrences show `transparent`.

- [ ] **Step 5: Visual check**

Reload `http://localhost:8081/` in dark and light. Colors have shifted to the new neutrals + magenta. (Cyan/violet still appear on Nexus/Tools/AI sections — fixed in Task 3.)

- [ ] **Step 6: Commit**

```bash
git add styles.css
git commit -m "style(redesign): repoint palette to Quiet tech tokens"
```

---

## Task 2: Remove the AI tells (gradients, halo, blur, pulse)

Strip the gradient-filled headline, hero halos, glassy blur, pulse animation, the gradient top-bars on cards, and remaining surface gradients.

**Files:**
- Modify: `styles.css` — `.nav` (~117), `.nav .pulse` + `@keyframes pulse` (~134–143), `.hero-halo` (~205–215), `.hero h1` (~237–250), `.spotlight-body::before` (~427–433), `.tool-tile::before` (~571–577), `.hero-image.kt`/`.hero-image.nexus` (~1020–1021), `.flow-node` bg (~1494) + light override (~1575–1576), `.ai-callout::before` (~1410–1416), `.prose p em` (~1146–1150)

- [ ] **Step 1: Define the checks**

```bash
grep -nE 'backdrop-filter|background-clip|@keyframes pulse' styles.css   # expect: no output
grep -nc 'linear-gradient\|radial-gradient' styles.css                  # expect: 0
```
Visual: hero headline is solid (not pink-faded); no glow behind hero; nav is solid; brand dot no longer pulses.

- [ ] **Step 2: Solidify the hero headline**

In `.hero h1`, remove the gradient-text declarations (`background: linear-gradient(...)`, `-webkit-background-clip: text`, `background-clip: text`, `color: transparent`) and set:

```css
  color: var(--text);
```
(Keep position/font-family/size/weight/letter-spacing/line-height/margin/max-width for now; type is tuned in Task 4.)

- [ ] **Step 3: Remove the hero halo rule**

Delete the entire `.hero-halo { ... }` rule and the `@media (prefers-color-scheme: light) { .hero-halo { opacity: 0.45; } }` block. (The empty `<div class="hero-halo">` in `index.html` is removed in Task 7.)

- [ ] **Step 4: De-glass the nav**

In `.nav`, remove `backdrop-filter: blur(12px);` and `-webkit-backdrop-filter: blur(12px);`. The `background: var(--nav-bg);` (now solid) and `border-bottom` stay.

- [ ] **Step 5: Make the brand dot static**

Replace `.nav .pulse` with (drop the glow + animation):

```css
.nav .pulse {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--accent);
}
```
Delete the `@keyframes pulse { ... }` block entirely.

- [ ] **Step 6: Remove the gradient top-bars**

Delete these three pseudo-element rules in full: `.spotlight-body::before`, `.tool-tile::before`, `.ai-callout::before`.

- [ ] **Step 7: Flatten remaining surface gradients**

- `.hero-image.kt` and `.hero-image.nexus`: replace each `background: linear-gradient(...)` with `background: var(--surface);`
- `.flow-node`: replace `background: linear-gradient(180deg, rgba(255,255,255,0.025), rgba(255,255,255,0));` with `background: var(--surface);` and delete the light-mode `.flow-node { background: linear-gradient(...) }` override.
- `.prose p em`: replace `background: linear-gradient(180deg, transparent 60%, var(--accent-soft) 60%);` with `background: var(--accent-soft);`

- [ ] **Step 8: Run the grep checks**

```bash
grep -nE 'backdrop-filter|background-clip|@keyframes pulse' styles.css
grep -c 'linear-gradient' styles.css
grep -c 'radial-gradient' styles.css
```
Expected: first command no output; gradient counts `0`. If a count is not `0`, locate the remaining `*-gradient(` and flatten it to `var(--surface)` or a solid color, then re-run.

- [ ] **Step 9: Visual check**

Reload dark + light. Headline solid, no glow, nav solid, dot static.

- [ ] **Step 10: Commit**

```bash
git add styles.css
git commit -m "style(redesign): remove gradients, halo, blur, and pulse animation"
```

---

## Task 3: Recolor component accents to neutral + sparing magenta

Repoint every cyan/violet component reference to neutral tokens, keep magenta only for state/interaction, and collapse status colors to the single magenta dot.

**Files:**
- Modify: `styles.css` — `.spotlight-body`, `.spotlight-monogram`, `.spotlight-tag`, `.spotlight-status`, `.spotlight-link`, `.spotlight-visual` (~411–530); `.tool-tile:hover`, `.tool-tile .monogram`, `.module-tag`, `.status-running` (~578–651); `.lab-tile` (dashed) + `.lab-tile.neon-arcade` (~664, 710–715); `.module-card` (~728–742); `.arcade-game-inspo` (~770–778); `.ship-tile .status.live/.soon` (~396–402) + light override (~399–402); `.title-block .meta-grid .live/.running/.soon` (~982–984) + light (~1001–1005); `.flow-eyebrow`, `.flow-node.gate`, `.gate-tag`, `.mini-gate-badge` (~1534–1572); `.ai-callout` (~1402–1409); `.callout-footer` (~1461–1479)

- [ ] **Step 1: Define the checks**

Visual: Nexus spotlight, Tools, Module strip, Arcade list, AI callout, and About flow all read neutral (hairline borders, magenta only on links/status). All status dots are magenta.

- [ ] **Step 2: Neutralize the Nexus spotlight**

```css
.spotlight-body { border: 0.5px solid var(--rule); }            /* was cyan */
.spotlight-monogram {
  background: var(--surface);
  border: 0.5px solid var(--rule-strong);
  color: var(--text);                                            /* was cyan */
}
.spotlight-tag {
  border: 0.5px solid var(--rule-strong);
  color: var(--text-muted);
  background: var(--surface);                                    /* was cyan */
}
.spotlight-status { color: var(--accent); }                      /* magenta status */
.spotlight-link {
  color: var(--accent);
  border-bottom: 1px solid var(--accent-soft);                   /* was cyan */
}
.spotlight-visual { border-left: 0.5px solid var(--rule); }      /* was cyan */
```
Also update the `@media (max-width: 900px) .spotlight-visual { border-top: ... }` cyan value to `0.5px solid var(--rule)`.

- [ ] **Step 3: Neutralize Tools**

```css
.tool-tile:hover, .tool-tile:focus-visible { border-color: var(--rule-strong); }  /* was violet */
.tool-tile .monogram {
  background: var(--surface);
  border: 0.5px solid var(--rule-strong);
  color: var(--text);                                            /* was violet */
}
.tool-tile .module-tag {
  border: 0.5px solid var(--rule-strong);
  color: var(--text-muted);
  background: var(--surface);                                    /* was violet */
}
.tool-tile .status-running { color: var(--accent); }             /* magenta */
```

- [ ] **Step 4: Lab tiles — solid border, keep one magenta pop**

```css
.lab-tile { border: 0.5px solid var(--rule-strong); }            /* was dashed */
.lab-tile:hover, .lab-tile:focus-visible {
  border-color: var(--rule-strong);
  background: var(--surface);                                    /* was cyan tint */
}
.lab-tile.neon-arcade {
  border-color: var(--accent);
  background: var(--accent-soft);
}
.lab-tile.neon-arcade .label { color: var(--accent); }
.lab-tile.neon-arcade .bottom { color: var(--accent); opacity: 0.7; }
```

- [ ] **Step 5: Module strip + Arcade list**

```css
.module-card {
  border: 0.5px solid var(--rule-strong);
  background: var(--surface);                                    /* was violet */
}
.module-card .module-name { color: var(--accent); }
.arcade-game-inspo { color: var(--accent); }                     /* was cyan */
```

- [ ] **Step 6: Collapse status colors to magenta**

```css
.ship-tile .status.live { color: var(--accent); }
.ship-tile .status.soon { color: var(--accent); }
.title-block .meta-grid .live   { color: var(--accent); }
.title-block .meta-grid .running{ color: var(--accent); }
.title-block .meta-grid .soon   { color: var(--accent); }
```
Delete the two `@media (prefers-color-scheme: light)` blocks that override `.status.live/.soon` and `.meta-grid .live/.running/.soon` with green/amber hexes (no longer needed — magenta works in both modes).

- [ ] **Step 7: Neutralize the About flow + gate accents**

```css
.flow-eyebrow { color: var(--text-muted); }                      /* was violet */
.flow-node.gate { background: var(--surface); border-color: var(--rule-strong); }
.flow-node.gate .flow-eyebrow { color: var(--accent); }
.flow-node.gate::after { border-left-color: var(--rule-strong); }
.gate-tag {
  color: var(--accent);
  border: 1px solid var(--accent-soft);
  background: var(--accent-soft);
}
```
(`.mini-gate-badge` is removed with the mini-flow in Task 7 — skip it here.)

- [ ] **Step 8: Neutralize the AI callout container + footer**

```css
.ai-callout {
  border: 0.5px solid var(--rule-strong);
  background: var(--surface);                                    /* was violet */
}
.callout-footer { border-top: 0.5px solid var(--rule); }
.callout-footer a { color: var(--accent); }
```

- [ ] **Step 9: Literal sweep deferred**

Do NOT grep for cyan/violet literals here — the `.mini-gate-badge` rule still carries `rgba(0,200,255,…)` until it is deleted in Task 7, and `.mini-*` still reference `var(--accent-violet)`. The global literal/variable sweep runs in Task 9, after Tasks 7 and 8. This step is visual only.

- [ ] **Step 10: Visual check**

Reload and check `/` (Nexus spotlight + Lab grid, neon-arcade tile is the one magenta pop), `/nexus/` (module strip), `/neon-arcade/` (arcade list), `/about/` (flow diagram) — all neutral with magenta only on links/status. Both modes.

- [ ] **Step 11: Commit**

```bash
git add styles.css
git commit -m "style(redesign): recolor cyan/violet accents to neutral + magenta"
```

---

## Task 4: Hero typography

Dial the hero headline back from shouty to restrained.

**Files:**
- Modify: `styles.css` — `.hero h1` (~237–250)

- [ ] **Step 1: Define the check**

Visual: hero "Ryan Saunders" is noticeably less heavy/tight and smaller at the top end, still clearly the focal point.

- [ ] **Step 2: Edit `.hero h1`**

Set these declarations (keep `color: var(--text)` from Task 2):

```css
  font-size: clamp(40px, 6vw, 60px);
  font-weight: 700;
  letter-spacing: -0.02em;
  line-height: 1.0;
```

- [ ] **Step 3: Visual check**

Reload `/` dark + light; confirm restrained headline.

- [ ] **Step 4: Commit**

```bash
git add styles.css
git commit -m "style(redesign): tune hero headline to restrained weight and size"
```

---

## Task 5: Casing and mono scope

Remove ALL-CAPS labels site-wide and reserve the mono font for system-style captions only.

**Files:**
- Modify: `styles.css` — every rule containing `text-transform: uppercase`; and `font-family: var(--font-mono)` on non-caption labels

- [ ] **Step 1: Define the checks**

```bash
grep -nc 'text-transform: uppercase' styles.css   # expect: 0
```
Visual: nav links, section headers, tile footers, eyebrows, buttons, crumbs all read in sentence/normal case.

- [ ] **Step 2: Remove every `text-transform: uppercase`**

Delete the `text-transform: uppercase;` declaration from each rule that has it. Known occurrences (selectors): `.nav .links`, `.hero .eyebrow`, `.hero .meta`, `.sec-head h2`, `.sec-head .sec-note`, `.ship-tile .tile-foot`, `.spotlight-tag`, `.spotlight-foot`, `.spotlight-link`, `.tool-tile .module-tag`, `.tool-tile .tile-foot`, `.module-card .module-name`, `.arcade-game-inspo`, `.arcade-game-meta`, `.about-teaser`/`.read-more`, `.btn`, `.contact .form-row label`, `.crumb`, `.title-block .eyebrow`, `.title-block .meta-grid`, `.next-nav .label`, `.foot`, `.prose h2`, `.privacy-content h2`/`.contact-content h2`, `.last-updated`, `.back-link`, `.form-group label`, `.submit-button`, `.flow-eyebrow`, `.gate-tag`, `.mini-gate-badge`. (Search to confirm none remain.)

- [ ] **Step 3: Narrow mono to captions only**

The mono font (`var(--font-mono)`) is kept ONLY on: `.sec-head .num`, `.hero .eyebrow`, `.hero .meta`, the tile/case-study status foots (`.ship-tile .tile-foot`, `.spotlight-foot`, `.tool-tile .tile-foot`, `.arcade-game-meta`), `.foot`, and `.title-block .meta-grid`. Change `font-family: var(--font-mono)` to `font-family: var(--font-text)` on the non-caption labels: `.crumb`, `.title-block .eyebrow`, `.next-nav .label`, `.stack .pill`, `.contact .form-row label`, `.form-group label`, `.last-updated`, `.module-card .module-name`, `.callout-footer a`. Reduce any `letter-spacing` above `0.08em` on remaining labels to `0.04em`.

- [ ] **Step 4: Run the grep check**

```bash
grep -nc 'text-transform: uppercase' styles.css
```
Expected: `0`.

- [ ] **Step 5: Visual check**

Reload `/`, `/integenius/`, `/about/` dark + light; labels are sentence case, mono only on the caption roles above.

- [ ] **Step 6: Commit**

```bash
git add styles.css
git commit -m "style(redesign): sentence-case labels and scope mono to captions"
```

---

## Task 6: Motion and hover

Remove the lift transforms; keep subtle color transitions.

**Files:**
- Modify: `styles.css` — `.ship-tile:hover`, `.spotlight-body:hover`, `.tool-tile:hover`, `.btn.primary:hover`, `.back-link`/`.submit-button` hovers

- [ ] **Step 1: Define the check**

Visual: hovering a card changes its border color but it does **not** translate upward; reduced-motion still respected.

- [ ] **Step 2: Remove `transform: translateY(...)` on hover**

In `.ship-tile:hover` and `.ship-tile:has(.tile-overlay-link:focus-visible)`, `.spotlight-body:hover` and its `:has(...)`, and `.tool-tile:hover`/`:focus-visible`: delete the `transform: translateY(-2px);` line. Keep the `border-color` change.

Also remove `transform: translateY(-1px);` from `.btn.primary:hover` and `.submit-button:hover`. Keep `background`/`border-color` transitions. Update the `transition:` properties on these rules to drop `transform` (leave `border-color`/`background`/`color`).

- [ ] **Step 3: Visual check**

Reload `/`; hover the Shipped tiles and Nexus spotlight — border shifts, no lift. Toggle DevTools "Emulate prefers-reduced-motion: reduce" and confirm no transitions.

- [ ] **Step 4: Commit**

```bash
git add styles.css
git commit -m "style(redesign): remove hover lift transforms"
```

---

## Task 7: Homepage content trims + remove dead mini-flow CSS

Trim the hero meta, add a tagline, remove the halo div, and replace the AI mini-pipeline with one quiet line. Then delete the now-unused `.mini-*` CSS.

**Files:**
- Modify: `index.html` — `.hero` block (~26–38), `.about-teaser`/`.ai-callout` block (~144–173)
- Modify: `styles.css` — delete `.mini-flow`, `.mini-node`, `.mini-node .tool-name`, `.mini-node .tool-role`, `.mini-node.placeholder-node`, `.mini-arrow`, `.mini-gate-badge` rules (~1417–1460)

- [ ] **Step 1: Define the checks**

Visual: hero shows eyebrow + name + one tagline line + one short meta line (no 5-item stat row). The "AI Stuff" section is a single sentence with an "About →" link (no pipeline diagram). No console errors.
```bash
grep -nc 'mini-flow\|mini-node\|mini-arrow\|mini-gate-badge' styles.css   # expect: 0
grep -nc 'hero-halo' index.html                                          # expect: 0
```

- [ ] **Step 2: Edit the hero in `index.html`**

Remove the `<div class="hero-halo" aria-hidden="true"></div>` line. Replace the `.meta` block (the five `<span>` stats) with a single tagline + one meta line:

```html
  <section class="hero" id="home">
    <div class="eyebrow"><span class="chip">v26</span>iOS · tools · systems</div>
    <h1>Ryan Saunders</h1>
    <p class="lede">I build small, useful iOS apps — and the tools that ship them.</p>
    <div class="meta"><span><b>3</b> shipped</span><span>Canada / remote</span></div>
  </section>
```

- [ ] **Step 3: Edit the AI section in `index.html`**

Replace the entire `.about-teaser` / `.ai-callout` block (the `<div class="ai-callout">` with the `.mini-flow`) with a single quiet callout:

```html
  <section class="about-teaser">
    <div class="ai-callout">
      <div class="callout-footer">
        <p>Multi-agent workflow, human-reviewed — I read every diff.</p>
        <a href="/about/">About →</a>
      </div>
    </div>
  </section>
```

- [ ] **Step 4: Delete the dead `.mini-*` CSS**

In `styles.css`, delete the `.mini-flow`, `.mini-node`, `.mini-node .tool-name`, `.mini-node .tool-role`, `.mini-node.placeholder-node`, `.mini-arrow`, and `.mini-gate-badge` rules.

- [ ] **Step 5: Run the grep checks**

```bash
grep -nc 'mini-flow\|mini-node\|mini-arrow\|mini-gate-badge' styles.css
grep -nc 'hero-halo' index.html
```
Expected: both `0`.

- [ ] **Step 6: Visual check**

Reload `/` dark + light. Hero trimmed with tagline; AI section is one line + link.

- [ ] **Step 7: Commit**

```bash
git add index.html styles.css
git commit -m "style(redesign): trim homepage hero and AI section copy"
```

---

## Task 8: Remove unused cyan/violet variables

All references are gone (Tasks 2, 3, 7), so delete the dead variables.

**Files:**
- Modify: `styles.css` — `:root` dark block and light block

- [ ] **Step 1: Define the check**

```bash
grep -nE 'accent-cyan|accent-violet' styles.css   # expect: no output
```

- [ ] **Step 2: Delete the variable definitions**

Remove these lines from BOTH the dark `:root` and the light `:root` blocks: `--accent-cyan`, `--accent-cyan-soft`, `--accent-violet`, `--accent-violet-soft`, `--accent-violet-border`.

- [ ] **Step 3: Run the grep check**

```bash
grep -nE 'accent-cyan|accent-violet' styles.css
```
Expected: no output.

- [ ] **Step 4: Visual check**

Reload `/`, `/nexus/`, `/about/` dark + light — nothing changed visually (the vars were already unreferenced). No broken colors.

- [ ] **Step 5: Commit**

```bash
git add styles.css
git commit -m "style(redesign): remove unused cyan and violet variables"
```

---

## Task 9: Full verification sweep

Confirm the whole site is coherent across page types and both modes, then fix and commit anything that slipped.

**Files:**
- Modify (only if a check fails): `styles.css`, `about/index.html`

- [ ] **Step 1: Static assertions**

```bash
cd /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders
grep -nc 'linear-gradient\|radial-gradient' styles.css        # 0
grep -nE 'backdrop-filter|background-clip|@keyframes pulse' styles.css   # none
grep -nc 'text-transform: uppercase' styles.css               # 0
grep -nE 'accent-cyan|accent-violet|#00c8ff|#a78bfa' styles.css          # none
grep -nE '#30d158|#ff9f0a|#1a7f3e|#a85d00|#6d28d9' styles.css            # none (old status colors)
```
All must pass. If any fails, fix the offending rule and re-run.

- [ ] **Step 2: Visual matrix — dark AND light each**

Visit and eyeball each, in both color schemes, via the local server:
- `/` — hero, Shipped grid, Nexus spotlight, Lab grid (neon-arcade = the one magenta pop), AI line, footer
- `/integenius/` — case-study template (title-block, prose, stack pills, next-nav)
- `/kitchensynk/` — hero-reveal slider still works and divider/handle visible in both modes
- `/nexus/` — module strip
- `/neon-arcade/` — arcade list neutral + magenta
- `/lcars/` — clean Quiet tech; identity in copy; verify prose still reads well (trim only if visibly verbose)
- `/about/` — flow diagram recolored, legible
- `/integenius/privacy/` and `/kitchensynk/contact/` — legacy template inherits palette; links magenta; form focus ring magenta

Confirm for each: no neon, no glow, no gradient, sentence case, magenta used only on links/status/focus/the one arcade tile.

- [ ] **Step 3: Contrast spot-check**

Confirm magenta link/text passes AA (≥ 4.5:1) in both modes on a representative page (e.g. `.spotlight-link` on `/`, a prose link on `/about/`). Use a contrast checker on the rendered colors. If any fails, darken the light `--accent` toward `#9E2059` or lighten the dark `--accent` toward `#E06B98` and re-check.

- [ ] **Step 4: Accessibility checks**

- Tab through `/` — focus-visible magenta ring appears on links, tiles, form fields.
- Emulate `prefers-reduced-motion: reduce` — no transitions fire.

- [ ] **Step 5: Commit any fixes**

```bash
git add -A
git commit -m "style(redesign): verification fixes across page types"
```
(If no fixes were needed, skip the commit.)

---

## Self-Review Notes (author)

- **Spec coverage:** palette (T1), de-neon (T2), recolor + status collapse (T3), typography (T4), casing + mono scope (T5), motion (T6), content trims (T7), var cleanup (T8), themed pages + accessibility + all 18 page types (T9). All spec sections map to a task.
- **No dangling `var()`:** cyan/violet vars defined through T7, removed in T8 after all references gone.
- **Themed pages:** neon-arcade magenta pop set in T3 Step 4; lcars verified clean in T9.
- **Out of scope (per spec):** real LCARS/arcade skins; a blog.
