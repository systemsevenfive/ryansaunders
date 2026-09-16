# Desk Accessory — Inner Pages (Pass 2) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Extend the Desk Accessory design to the 17 inner pages of `ryansaunders-copy` — append inner-page component styles to `styles.css` and unify the shared masthead + footer markup — so the whole site reads as one.

**Architecture:** One append to `styles.css` (inner-page component section; homepage section untouched). A scripted replace of the shared `<nav class="nav">` and `<footer class="foot">` blocks on all 17 pages with the homepage masthead/footer. All other content markup (title blocks, prose, figures, forms, one-offs) is kept and styled purely through CSS. The `hero-reveal` slider JS on kitchensynk/nexus is preserved.

**Tech Stack:** Plain HTML5 + one CSS file. Verification via `python3 -m http.server` + the Browser pane.

**Medium adaptations:** No test runner; "verify" steps are `grep` asserts + browser render checks. No `.git` in the copy; "checkpoint" replaces commit. Spec: `docs/superpowers/specs/2026-09-13-desk-accessory-inner-pages-design.md`.

---

## File structure

- `ryansaunders-copy/styles.css` — **append** the inner-page section. Owns: `.page` container, crumb, title-block, meta-grid, hero/slider, prose, pills, feature-list, callout, next-nav, module-strip, arcade cards, flow diagram, privacy/contact containers, forms, back-links.
- `ryansaunders-copy/<page>/index.html` × 17 — **masthead + footer markup only** replaced. Content markup untouched.

---

### Task 1: Append the inner-page component styles

**Files:**
- Modify (append to end): `ryansaunders-copy/styles.css`

- [ ] **Step 1: Append this block verbatim to the end of `styles.css`**

```css

/* ==========================================================================
   Inner pages (Pass 2) — case study, about, privacy, contact.
   Homepage section above is untouched; these style the existing page markup.
   Spec: docs/superpowers/specs/2026-09-13-desk-accessory-inner-pages-design.md
   ========================================================================== */

.page{max-width:720px; margin:0 auto; padding:34px clamp(18px,5vw,28px) 56px;}
.num{display:none;}

/* crumb */
.crumb{font-family:var(--mono); font-size:12px; color:var(--dim); margin:0 0 22px;}
.crumb a{color:var(--dim);}

/* title block */
.title-block{margin:0 0 8px;}
.title-block .eyebrow{font-family:var(--mono); font-size:11.5px; letter-spacing:.14em; text-transform:uppercase; color:var(--dim); margin:0 0 8px;}
.title-block h1{margin:0; font-size:clamp(26px,6vw,34px); line-height:1.08; letter-spacing:-.015em; text-wrap:balance;}
.title-block .tag{margin:12px 0 0; color:var(--dim); max-width:60ch;}

/* meta grid */
.meta-grid{display:flex; flex-wrap:wrap; gap:14px 30px; margin:20px 0 0; padding:14px 0 0; border-top:1px solid var(--line);}
.meta-grid > div{display:flex; flex-direction:column; font-family:var(--mono); font-size:11px; letter-spacing:.1em; text-transform:uppercase; color:var(--dim);}
.meta-grid b{font-family:var(--sans); font-weight:600; font-size:14.5px; letter-spacing:0; text-transform:none; color:var(--ink);}
.meta-grid .live{color:var(--accent);}
.platforms{display:inline-flex; align-items:center; gap:6px;}
.platform-icon{height:15px; width:auto; color:var(--ink);}

/* hero image + before/after slider */
.hero-image{margin:26px 0 6px;}
.hero-image img, .hero-screenshot{display:block; width:100%; border:1px solid var(--line-2);}
.hero-image figcaption{font-family:var(--mono); font-size:12px; color:var(--dim); margin:8px 0 0;}
.hero-image:has(.hero-reveal){display:block; background:none;}
.hero-reveal{position:relative; aspect-ratio:var(--hero-reveal-ratio,2752/1104); width:100%; overflow:hidden;
  user-select:none; -webkit-user-select:none; touch-action:pan-y; cursor:ew-resize; --handle-size:32px; border:1px solid var(--line-2);}
.hero-reveal__layer{position:absolute; inset:0; display:block;}
.hero-reveal__layer img{width:100%; height:100%; object-fit:cover; display:block; pointer-events:none; -webkit-user-drag:none; border:none;}
.hero-reveal__top{clip-path:inset(0 0 0 calc(var(--reveal) * 100%));}
.hero-reveal__divider{position:absolute; top:0; bottom:0; left:calc(var(--reveal) * 100%); width:1px; margin-left:0; background:var(--ink); pointer-events:none;}
.hero-reveal__handle{position:absolute; top:50%; left:calc(var(--reveal) * 100%); width:var(--handle-size); height:var(--handle-size);
  margin:calc(var(--handle-size) / -2) 0 0 calc(var(--handle-size) / -2); border:1px solid var(--ink); background:var(--paper); color:var(--ink);
  cursor:ew-resize; padding:0; display:grid; place-items:center; touch-action:none; font-family:var(--mono); font-size:12px; border-radius:6px;
  box-shadow:inset 1px 1px 0 var(--btn-hi), inset -1px -1px 0 var(--btn-sh);}
.hero-reveal__handle::before{content:"◄►";}
.hero-reveal__handle-icon{display:none;}
.hero-reveal__handle:focus-visible{outline:2px solid var(--accent); outline-offset:3px;}
.hero-reveal[data-dragging="true"], .hero-reveal[data-dragging="true"] .hero-reveal__handle{cursor:grabbing;}

/* prose */
.prose p{max-width:65ch; margin:0 0 14px;}
.prose h2{display:flex; align-items:center; gap:10px; font-family:var(--mono); font-weight:400; font-size:11.5px; letter-spacing:.16em;
  text-transform:uppercase; color:var(--dim); margin:34px 0 12px;}
.prose h2::after{content:""; flex:1; height:1px; background:var(--line);}
.prose h3{font-size:16px; margin:18px 0 4px;}
.prose ul{list-style:none; max-width:65ch; margin:12px 0; padding:0;}
.prose ul li{position:relative; padding:3px 0 3px 18px;}
.prose ul li::before{content:""; position:absolute; left:2px; top:12px; width:6px; height:6px; background:var(--dim);}

/* pills */
.stack{display:flex; flex-wrap:wrap; gap:8px; margin:12px 0;}
.pill{font-family:var(--mono); font-size:12px; color:var(--dim); border:1px solid var(--line-2); border-radius:20px; padding:2px 11px;}

/* feature list */
.feature-list{list-style:none; margin:8px 0; padding:0; border-top:1px solid var(--line);}
.feature-list li{position:relative; padding:9px 0 9px 20px; border-bottom:1px solid var(--line);}
.feature-list li::before{content:""; position:absolute; left:2px; top:16px; width:7px; height:7px; border:1px solid var(--accent);}

/* callout */
.callout{border:1px dashed var(--line-2); border-radius:4px; padding:14px 16px; margin:18px 0; color:var(--dim); font-size:15px;}

/* next-nav */
.next-nav{display:flex; justify-content:space-between; gap:16px; flex-wrap:wrap; margin:34px 0 0; padding:16px 0 0; border-top:1px solid var(--line);}
.next-nav a{text-decoration:none; color:var(--ink);}
.next-nav a:last-child{text-align:right;}
.next-nav .label{display:block; font-family:var(--mono); font-size:11px; letter-spacing:.1em; text-transform:uppercase; color:var(--dim); margin-bottom:3px;}
.next-nav .title{font-weight:600;}
.next-nav a:hover .title{color:var(--accent); text-decoration:underline;}

/* nexus module strip */
.module-strip{display:grid; grid-template-columns:1fr; gap:12px; margin:18px 0;}
@media (min-width:620px){ .module-strip{grid-template-columns:repeat(auto-fit,minmax(120px,1fr));} }
.module-card{border:1px solid var(--line); border-radius:4px; padding:10px 12px;}
.module-card .module-name{font-weight:600;}
.module-card .module-desc{color:var(--dim); font-size:13px;}

/* neon-arcade game cards */
.arcade-games{display:grid; grid-template-columns:1fr; gap:14px; margin:18px 0;}
@media (min-width:620px){ .arcade-games{grid-template-columns:1fr 1fr;} }
.arcade-game{border:1px solid var(--line); border-radius:4px; padding:16px;}
.arcade-game-inspo{font-family:var(--mono); font-size:12px; color:var(--dim);}
.arcade-game-title{font-weight:600; font-size:16px; margin:2px 0;}
.arcade-game-desc{color:var(--dim);}
.arcade-game-meta{margin-top:12px; padding-top:8px; border-top:1px solid var(--line); font-family:var(--mono); font-size:12px; color:var(--dim);}

/* about — AI flow diagram */
.flow, .flow-detail{display:flex; align-items:stretch; gap:10px; flex-wrap:wrap; margin:16px 0;}
.flow-node{border:1px solid var(--line); border-radius:4px; padding:8px 12px; flex:1 1 0; min-width:110px;}
.flow-node .tool-name{font-weight:600; display:block;}
.flow-node .tool-role{color:var(--dim); font-size:12px; font-family:var(--mono);}
.flow-placeholder{border-style:dashed;}
.flow-placeholder .tool-name{color:var(--dim);}
.flow-arrow{display:flex; align-items:center; color:var(--dim); font-family:var(--mono); flex:none;}
.flow-arrow::before{content:none;}
.flow-arrow::after{content:"→";}
.gate-badge{background:var(--ink); color:var(--paper); padding:0 6px; font-size:12px; font-family:var(--mono);}
.flow-caption{color:var(--dim); margin-top:12px; font-size:14px;}
@media (max-width:620px){
  .flow, .flow-detail{flex-direction:column;}
  .flow-arrow{justify-content:center;}
  .flow-arrow::after{content:"↓";}
}

/* privacy + contact containers */
.privacy-container, .contact-container{max-width:65ch; margin:0 auto;}
.privacy-content h1, .contact-content h1{margin:0 0 14px; font-size:clamp(24px,6vw,32px); line-height:1.1; letter-spacing:-.015em; text-wrap:balance;}
.privacy-content h2{display:flex; align-items:center; gap:10px; font-family:var(--mono); font-weight:400; font-size:11.5px; letter-spacing:.16em;
  text-transform:uppercase; color:var(--dim); margin:28px 0 10px;}
.privacy-content h2::after{content:""; flex:1; height:1px; background:var(--line);}
.privacy-content h3{font-size:15px; margin:16px 0 4px;}
.privacy-content p{margin:0 0 12px; max-width:65ch;}
.privacy-content ul{list-style:none; padding:0; margin:12px 0; max-width:65ch;}
.privacy-content ul li{position:relative; padding:3px 0 3px 18px;}
.privacy-content ul li::before{content:""; position:absolute; left:2px; top:12px; width:6px; height:6px; background:var(--dim);}
.last-updated{font-family:var(--mono); font-size:11.5px; color:var(--dim);}
.button-container{display:flex; flex-wrap:wrap; gap:8px 16px; margin:0 0 24px;}
.button-container--footer{margin:28px 0 0; padding-top:14px; border-top:1px solid var(--line);}
.back-link{font-family:var(--mono); font-size:12px; color:var(--dim);}

/* contact form */
.contact-content p{margin:0 0 14px; max-width:65ch; color:var(--dim);}
.contact-form{margin-top:16px; max-width:60ch;}
.form-group{margin:0 0 14px;}
.contact-form label{display:block; font-family:var(--mono); font-size:11px; letter-spacing:.1em; text-transform:uppercase; color:var(--dim); margin:0 0 5px;}
.contact-form input, .contact-form textarea{width:100%; font-family:var(--sans); font-size:15px; background:var(--paper); color:var(--ink);
  border:1px solid var(--line-2); border-radius:6px; padding:8px 10px;}
.contact-form textarea{min-height:120px; resize:vertical;}
.contact-form input:focus, .contact-form textarea:focus{outline:2px solid var(--accent); outline-offset:1px; border-color:var(--accent);}
.submit-button{display:inline-block; margin-top:6px; font-family:var(--mono); font-size:12.5px; letter-spacing:.04em; color:var(--ink); cursor:pointer;
  background:var(--paper); border:1px solid var(--line-2); border-radius:7px; padding:8px 16px;
  box-shadow:inset 1px 1px 0 var(--btn-hi), inset -1px -1px 0 var(--btn-sh);}
.submit-button:hover{border-color:var(--dim);}
```

- [ ] **Step 2: Assert the append landed and stays self-contained**

Run: `cd ~/Desktop/Dev/Websites/ryansaunders-copy && grep -c "Inner pages (Pass 2)" styles.css`
Expected: `1`.

Run: `grep -nE "@import|fonts\.googleapis|url\(" styles.css`
Expected: **no output** — still zero external resources.

- [ ] **Step 3: Checkpoint** — proceed to chrome unification; visual verification is Task 3.

---

### Task 2: Unify masthead + footer markup across the 17 inner pages

**Files:**
- Modify: all 17 `ryansaunders-copy/**/index.html` except the root `index.html`.

Each page currently opens with `<nav class="nav" ...>…</nav>` and ends with `<footer class="foot">…</footer>`. Replace both with the homepage blocks. The case-study pages also contain a second `<nav class="next-nav">` — the replace targets only the masthead nav (non-greedy match stops at the first `</nav>`), leaving next-nav intact.

- [ ] **Step 1: Run the block-replace script**

Run this exact script:

```bash
cd ~/Desktop/Dev/Websites/ryansaunders-copy
python3 - <<'PY'
import re, glob

MAST = '''<div class="top">
  <div class="wrap">
    <a class="brand" href="/">
      <span class="cbox" aria-hidden="true"></span>
      <span class="pin" aria-hidden="true"></span>
      Ryan Saunders
    </a>
    <nav class="links" aria-label="Primary">
      <a href="/#work">work</a>
      <a href="/#lab">lab</a>
      <a href="/about/">about</a>
      <a href="/#contact">contact</a>
    </nav>
  </div>
</div>'''

FOOT = '''<footer>
  <div class="wrap">
    <span>© 2026 · Ryan Saunders</span>
    <span>built plain · loads fast</span>
  </div>
</footer>'''

nav_re  = re.compile(r'<nav class="nav".*?</nav>', re.DOTALL)
foot_re = re.compile(r'<footer class="foot".*?</footer>', re.DOTALL)

for path in glob.glob('**/index.html', recursive=True):
    if path == 'index.html' or path.startswith('.superpowers/'):
        continue
    src = open(path, encoding='utf-8').read()
    new, n1 = nav_re.subn(MAST, src, count=1)
    new, n2 = foot_re.subn(FOOT, new, count=1)
    open(path, 'w', encoding='utf-8').write(new)
    print(f'{path}: nav={n1} footer={n2}')
PY
```

Expected: 17 lines, each `nav=1 footer=1`.

- [ ] **Step 2: Assert every inner page carries the new chrome and none of the old**

Run: `grep -rl 'class="nav"' --include=index.html . | grep -v '^./index.html' | grep -v .superpowers`
Expected: **no output** — no old masthead remains.

Run: `grep -rl 'class="foot"' --include=index.html . | grep -v .superpowers`
Expected: **no output** — no old footer remains.

Run: `grep -rlc 'class="top"' --include=index.html . | grep -v .superpowers | wc -l`
Expected: `18` (root homepage + 17 inner pages all have the `.top` masthead).

Run: `grep -c 'class="next-nav"' kitchensynk/index.html`
Expected: `1` — the case-study next-nav survived the masthead replace.

- [ ] **Step 3: Checkpoint** — proceed to render verification.

---

### Task 3: Render verification across page types

**Files:** none (verification only).

- [ ] **Step 1: Serve the copy**

Run (background): `cd ~/Desktop/Dev/Websites/ryansaunders-copy && python3 -m http.server 8091`
Open the Browser pane to `http://localhost:8091/`.

- [ ] **Step 2: Case study with slider — `/kitchensynk/`**

Open `http://localhost:8091/kitchensynk/` at desktop width, light. Screenshot.
Expected: new masthead; crumb; eyebrow + h1 + tag; meta grid (mono labels / bold values, `● live` in accent, platform icons); hero **before/after slider** with a beveled square handle; mono-label prose sections (no `00/01` numbers); pills; hairline feature list; App Store button; next-nav.

- [ ] **Step 3: Slider still works**

Drag the slider handle left/right (or via JS check the `--reveal` variable changes). Confirm the reveal moves.
Run in the browser console tool: `getComputedStyle(document.querySelector('.hero-reveal')).getPropertyValue('--reveal')` after a drag — expect a value other than the initial `0.5`.

- [ ] **Step 4: One-offs — `/nexus/` (module strip) and `/neon-arcade/` (game cards) and `/about/` (flow)**

Open each. Screenshot.
Expected: nexus module-strip = row of hairline cards; neon-arcade = two-column hairline game cards; about = flow-node boxes joined by `→`, `gate` badge inverted.

- [ ] **Step 5: Boilerplate — `/integenius/privacy/` and `/integenius/contact/`**

Open each. Screenshot.
Expected: privacy = h1 + mono-label sections + dim prose + back-links; contact = mono field labels, 1px inputs, beveled submit. (Copy is placeholder/blank — that is expected; styling is what's under test.)

- [ ] **Step 6: Dark + phone**

On `/kitchensynk/` set `colorScheme: dark`; then the mobile preset (375). Screenshot each.
Expected: warm dark palette throughout, all text legible; meta grid and flow stack cleanly; no horizontal overflow.

- [ ] **Step 7: No overflow, no external requests**

On `/kitchensynk/` run: `({o: document.documentElement.scrollWidth > document.documentElement.clientWidth})` — expect `o:false`.
Read network requests — expect only `localhost:8091` (document, `/styles.css`, and same-origin `/images/...`). No third-party hosts.

- [ ] **Step 8: Checkpoint** — fix any failure in `styles.css` and re-run the affected step before proceeding. Stop the server when done.

---

### Task 4: Handoff to Ryan

- [ ] **Step 1:** Summarize (point form): pages restyled, one-offs verified, slider working, dark/mobile checked. Point Ryan at the local server or a published artifact of a representative page. Pass 2 complete; next options are his (real copy pass, promote toward live site, or park).

---

## Notes for the implementer

- Do not touch the homepage section of `styles.css` or `index.html` (Pass 1, done).
- Do not edit content markup inside inner pages — only the masthead/footer blocks change (Task 2). Everything else is CSS.
- Preserve the `hero-reveal` `<script>` on kitchensynk and nexus exactly.
- Placeholder/blank copy (including empty form labels and submit text) stays — Ryan writes copy later.
- No web fonts, images-in-shell, or new JS.
