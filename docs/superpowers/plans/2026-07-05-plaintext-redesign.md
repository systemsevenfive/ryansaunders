# Plaintext Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Apply the approved Plaintext design (spec: `docs/superpowers/specs/2026-07-05-plaintext-redesign-design.md`) to the entire site: new `styles.css`, rebuilt homepage, restyled subpages.

**Architecture:** One shared `styles.css`, rewritten from scratch and assembled task-by-task (shared chrome first, then per-page-template component groups). Existing subpage markup is preserved wherever possible — the new CSS restyles the existing class names (`crumb`, `title-block`, `prose`, `next-nav`, `privacy-container`, …). Bracket link chrome and ASCII rules are generated in CSS (`::before`/`::after` glyph runs), so most pages need zero or tiny markup edits. Only `index.html` is rebuilt.

**Tech Stack:** Plain HTML + one CSS file. No build step, no web fonts, no new JS. The two existing hero-reveal sliders (kitchensynk, nexus) keep their inline JS, restyled.

**Working rules (from CLAUDE.md + memory):**
- NO pushes — this repo deploys on push. All commits stay local on `main`.
- Visual checkpoints: Ryan previews in browser BEFORE each commit. Serve the repo root: `python3 -m http.server 8891 --bind 127.0.0.1` (run once, background, from repo root) → http://localhost:8891
- All copy is placeholder — do not edit or "improve" copy beyond case changes the design requires.
- Reference for the approved look: scratchpad mockup `index.html` (this plan inlines its CSS; the scratchpad may be gone in a later session — the plan is self-contained).

---

### Task 1: Rewrite `styles.css` — tokens + shared chrome + homepage components

**Files:**
- Modify: `styles.css` (full replacement)

- [ ] **Step 1: Replace `styles.css` entirely with the base system**

```css
/* ==========================================================================
   Plaintext — ryansaunders.com
   Relaxed web 1.0: monospace, ASCII rules, 1px borders.
   Neon screenshots are the only saturated color on any page.
   ========================================================================== */

:root {
  --bg: #0a0a0a;
  --text: #c8c8c8;
  --text-dim: #7a7a7a;
  --rule: #444;
  --img-border: #444;
}
@media (prefers-color-scheme: light) {
  :root {
    --bg: #fafaf8;
    --text: #222;
    --text-dim: #6b6b6b;
    --rule: #bbb;
    --img-border: #999;
  }
}

* { box-sizing: border-box; margin: 0; padding: 0; }
html { background: var(--bg); scroll-behavior: smooth; }
body {
  font-family: "Courier New", Courier, monospace;
  font-size: 15px;
  line-height: 1.65;
  color: var(--text);
  background: var(--bg);
}

a { color: var(--text); text-decoration: underline; }
a:hover { background: var(--text); color: var(--bg); text-decoration: none; }

h1, h2, h3 { font-size: 15px; font-weight: bold; letter-spacing: 0.08em; }

/* ---- ASCII rules -------------------------------------------------------- */
.rule-heavy, .rule { overflow: hidden; }
.rule-heavy::before, .rule::before {
  display: block; white-space: nowrap; color: var(--rule); margin-bottom: 1em;
}
.rule-heavy::before { content: "================================================================================================================================================================================================"; }
.rule::before { content: "------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------"; }

/* ---- Sticky top bar (existing .nav markup on every page) ---------------- */
.nav {
  position: sticky;
  top: 0;
  z-index: 10;
  background: var(--bg);
  border-bottom: 1px dashed var(--rule);
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  gap: 1ch;
  flex-wrap: wrap;
  max-width: 1080px;
  margin: 0 auto;
  padding: 12px 20px;
}
/* full-bleed bar background behind the centered content */
@supports (width: 1px) {
  .nav { max-width: none; }
  .nav > * { }
}
.nav .brand {
  font-weight: bold;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  text-decoration: none;
}
.nav .pulse { display: none; }
.nav .links { word-spacing: 0.2em; }
.nav .links a { white-space: nowrap; text-transform: lowercase; }
.nav .links a::before { content: "["; }
.nav .links a::after { content: "]"; }
.nav .links a:hover::before, .nav .links a:hover::after { background: var(--bg); color: var(--text); }

/* ---- Page container ------------------------------------------------------ */
.page {
  max-width: 1080px;
  margin: 0 auto;
  padding: 32px 20px 48px;
}
section, article { scroll-margin-top: 72px; }

.page-head h1 { text-transform: uppercase; }
.tagline { margin-top: 0.4em; max-width: 72ch; }

h2 .num { color: var(--text-dim); margin-right: 1ch; }

/* Section h2s get an ASCII rule automatically */
main h2 { margin-top: 2.6em; overflow: hidden; }
main h2::after {
  content: "------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------";
  display: block; white-space: nowrap; color: var(--rule);
  font-weight: normal; letter-spacing: normal; margin-bottom: 0.6em;
}

/* ---- Cards (homepage shipped/lab; reused by subpage strips) -------------- */
.card-grid { display: grid; grid-template-columns: 1fr; gap: 16px; }
@media (min-width: 720px) { .card-grid { grid-template-columns: repeat(3, 1fr); } }

.ship-card {
  border: 1px solid var(--rule);
  padding: 18px 18px 14px;
  display: flex;
  flex-direction: column;
  position: relative;
}
.ship-card:hover { border-color: var(--text-dim); }
.ship-head { display: flex; align-items: center; gap: 12px; margin-bottom: 0.6em; }
.ship-head img { width: 44px; height: 44px; border: 1px solid var(--img-border); }
.ship-head h3 a { text-decoration: none; }
.ship-head h3 a::after { content: ""; position: absolute; inset: 0; }
.ship-card .sub { color: var(--text-dim); flex: 1; }
.ship-foot {
  display: flex; align-items: baseline; justify-content: space-between; gap: 1ch; flex-wrap: wrap;
  margin-top: 1em; padding-top: 0.6em;
  border-top: 1px dashed var(--rule);
  color: var(--text-dim);
}
.ship-foot .store { position: relative; z-index: 1; color: var(--text); }
.card-label { color: var(--text-dim); font-size: 13px; margin-bottom: 0.5em; }

/* ---- Entry lines with dotted leaders (nexus section) --------------------- */
.entry { margin: 1.1em 0; }
.entry-line { display: flex; align-items: baseline; gap: 0.75ch; flex-wrap: wrap; }
.entry-line .leader { flex: 1; min-width: 4ch; border-bottom: 1px dotted var(--text-dim); transform: translateY(-4px); }
.entry-line .status { color: var(--text-dim); }
.entry .sub { color: var(--text-dim); }

/* ---- Split (text | screenshot) ------------------------------------------- */
.split { display: grid; grid-template-columns: 1fr; gap: 0 3ch; align-items: start; }
@media (min-width: 900px) {
  .split { grid-template-columns: minmax(32ch, 5fr) 7fr; }
  .split figure { margin-top: 0.2em; }
}

/* ---- Figures -------------------------------------------------------------- */
figure { margin: 1.6em 0 2em; }
figure img { display: block; width: 100%; height: auto; border: 1px solid var(--img-border); }
figcaption { color: var(--text-dim); margin-top: 0.4em; font-size: 13px; }

/* ---- AI workflow flow (homepage + about) ---------------------------------- */
.flow { display: flex; align-items: center; gap: 10px; }
.flow-node {
  border: 1px solid var(--rule);
  padding: 8px 14px;
  display: flex;
  flex-direction: column;
  text-align: center;
  flex-shrink: 0;
}
.flow-node .tool-name { font-weight: bold; }
.flow-node .tool-role { color: var(--text-dim); font-size: 13px; }
.flow-placeholder { border-style: dashed; }
.flow-placeholder .tool-name { color: var(--text-dim); }
.flow-arrow { flex: 1; min-width: 4ch; display: flex; align-items: center; color: var(--text-dim); }
.flow-arrow::before {
  content: "------------------------------------------------------------------------------------------------";
  flex: 1; overflow: hidden; white-space: nowrap;
}
.flow-arrow::after { content: ">"; }
.gate-badge { background: var(--text); color: var(--bg); padding: 0 6px; font-size: 13px; }
.flow-caption { color: var(--text-dim); margin-top: 1em; }
@media (max-width: 719px) {
  .flow { flex-direction: column; align-items: stretch; }
  .flow-arrow { justify-content: center; min-width: 0; }
  .flow-arrow::before { content: "↓"; flex: none; }
  .flow-arrow::after { content: none; }
}

/* ---- Forms ---------------------------------------------------------------- */
form { margin-top: 1.2em; max-width: 60ch; }
label { display: block; color: var(--text-dim); margin-top: 1em; }
input, textarea {
  font-family: inherit; font-size: inherit;
  width: 100%;
  background: var(--bg); color: var(--text);
  border: 1px solid var(--rule);
  padding: 6px 8px;
  margin-top: 0.3em;
}
input:focus, textarea:focus { outline: 1px solid var(--text); }
button, .btn {
  font-family: inherit; font-size: inherit;
  display: inline-block;
  background: var(--bg); color: var(--text);
  border: 1px solid var(--text);
  padding: 6px 16px;
  margin-top: 1em;
  cursor: pointer;
  text-decoration: none;
}
button:hover, .btn:hover { background: var(--text); color: var(--bg); }
.btn.ghost { border-color: var(--rule); }

/* ---- Footer ---------------------------------------------------------------- */
.foot {
  max-width: 1080px;
  margin: 3.5em auto 0;
  padding: 0 20px 40px;
  color: var(--text-dim);
  display: flex;
  flex-wrap: wrap;
  gap: 0.5ch 3ch;
  overflow: hidden;
}
.foot::before {
  content: "------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------";
  display: block; white-space: nowrap; color: var(--rule);
  width: 100%; margin-bottom: 1em;
}
```

Note the `.nav` full-bleed handling: the simple version above centers the nav content but the dashed border spans the viewport because `.nav` itself is full width. Implement it exactly as: `.nav` full width + border, and an inner content constraint using `padding: 12px max(20px, calc((100% - 1040px) / 2))` instead of the `@supports` placeholder block shown above (delete that block; it exists here only to flag the issue):

```css
.nav {
  position: sticky;
  top: 0;
  z-index: 10;
  background: var(--bg);
  border-bottom: 1px dashed var(--rule);
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  gap: 1ch;
  flex-wrap: wrap;
  padding: 12px max(20px, calc((100% - 1040px) / 2));
}
```

- [ ] **Step 2: Verify CSS parses and tokens resolve**

Run: `python3 -c "import re,sys; css=open('/Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/styles.css').read(); assert css.count('{')==css.count('}'), 'brace mismatch'; print('braces ok', css.count('{'))"`
Expected: `braces ok <n>` with no assertion error.

Do NOT commit yet — Task 2 (homepage markup) lands with it at the first checkpoint.

---

### Task 2: Rebuild `index.html`

**Files:**
- Modify: `index.html` (full replacement)

- [ ] **Step 1: Replace `index.html`**

Keep: `<title>`, meta description, `/styles.css` link, Formspree action, all image paths, LinkedIn/App Store URLs. Structure per approved mockup, but using the shared `.nav` markup (brackets come from CSS) and `.card-grid` class:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ryan Saunders — iOS Developer</title>
  <meta name="description" content="iOS developer.">
  <link rel="stylesheet" href="/styles.css">
</head>
<body>

<nav class="nav" aria-label="Primary">
  <a href="/" class="brand">Ryan Saunders</a>
  <div class="links">
    <a href="#shipped">work</a>
    <a href="#nexus">nexus</a>
    <a href="#lab">lab</a>
    <a href="/about/">about</a>
    <a href="#contact">contact</a>
  </div>
</nav>

<main class="page">

  <header class="page-head">
    <h1>Ryan Saunders</h1>
    <div class="rule-heavy" aria-hidden="true"></div>
    <p class="tagline">iOS developer. Canada / remote.</p>
    <p class="tagline">I build small, useful iOS apps — and the tools that ship them.</p>
  </header>

  <section id="shipped">
    <h2>SHIPPED</h2>
    <div class="card-grid">
      <div class="ship-card">
        <div class="ship-head">
          <img src="/images/icons/integenius-80.png" alt="" aria-hidden="true">
          <h3><a href="/integenius/">integenius</a></h3>
        </div>
        <p class="sub">Arithmetic flash cards for the iPad generation.</p>
        <div class="ship-foot">
          <span>● live · v3 apr '26</span>
          <a class="store" href="https://apps.apple.com/ca/app/integenius/id6471911678" target="_blank" rel="noopener noreferrer">[app store]</a>
        </div>
      </div>
      <div class="ship-card">
        <div class="ship-head">
          <img src="/images/icons/kitchensynkv2-80.jpg" alt="" aria-hidden="true">
          <h3><a href="/kitchensynk/">kitchen synk</a></h3>
        </div>
        <p class="sub">An alternative to the built-in iOS Timer app, for those cooks using every burner.</p>
        <div class="ship-foot">
          <span>● live · v2 spring '26</span>
          <a class="store" href="https://apps.apple.com/us/app/kitchen-synk/id6755136814" target="_blank" rel="noopener noreferrer">[app store]</a>
        </div>
      </div>
      <div class="ship-card">
        <div class="ship-head">
          <img src="/images/icons/solitairezero-80.png" alt="" aria-hidden="true">
          <h3><a href="/solitairezero/">solitaire zero</a></h3>
        </div>
        <p class="sub">Because sometimes you just want to play solitaire with no fuss.</p>
        <div class="ship-foot">
          <span>● live · v1 2026</span>
          <a class="store" href="https://apps.apple.com/us/app/solitaire-zero/id6760957963" target="_blank" rel="noopener noreferrer">[app store]</a>
        </div>
      </div>
    </div>
    <figure>
      <picture>
        <source media="(prefers-color-scheme: light)" srcset="/images/kitchensynk/hero-light-color-scaled.jpg">
        <img src="/images/kitchensynk/hero-dark-color-scaled.jpg" alt="Kitchen Synk running four timers at once">
      </picture>
      <figcaption>fig 1. kitchen synk — every burner accounted for</figcaption>
    </figure>
  </section>

  <section id="nexus">
    <h2>NEXUS</h2>
    <div class="split">
      <div class="entry">
        <div class="entry-line">
          <a href="/nexus/">nexus</a>
          <span class="leader" aria-hidden="true"></span>
          <span class="status">built &amp; running</span>
        </div>
        <p class="sub">Homegrown project management for my AI workflows. Tickets, reports, live agent view.</p>
        <p class="sub">node.js · fastify · vanilla js</p>
      </div>
      <figure>
        <picture>
          <source media="(prefers-color-scheme: light)" srcset="/images/nexus/hero-light-sessions-scaled.jpg">
          <img src="/images/nexus/hero-dark-sessions-scaled.jpg" alt="Nexus sessions monitor: twelve agents in flight, color-coded by task age">
        </picture>
        <figcaption>fig 2. nexus session monitor — twelve agents in flight</figcaption>
      </figure>
    </div>
  </section>

  <section id="lab">
    <h2>IN THE LAB</h2>
    <div class="card-grid">
      <div class="ship-card">
        <p class="card-label">exp-001 · tool</p>
        <div class="ship-head">
          <h3><a href="/edassists/">ed assists</a></h3>
        </div>
        <p class="sub">Ed would like to Assist the video editors of the world.</p>
        <div class="ship-foot"><span>○ prototype</span></div>
      </div>
      <div class="ship-card">
        <p class="card-label">exp-004 · tool</p>
        <div class="ship-head">
          <h3><a href="/cardinal/">cardinal</a></h3>
        </div>
        <p class="sub">The missing link between Contacts and Calendars.</p>
        <div class="ship-foot"><span>○ tinkering</span></div>
      </div>
      <div class="ship-card">
        <p class="card-label">arcade · games</p>
        <div class="ship-head">
          <h3><a href="/neon-arcade/">the neon arcade</a></h3>
        </div>
        <p class="sub">Where I explore and pay homage to the classics, in Swift.</p>
        <div class="ship-foot"><span>○ in development</span></div>
      </div>
    </div>
  </section>

  <section id="ai">
    <h2>AI STUFF</h2>
    <div class="flow">
      <div class="flow-node"><span class="tool-name">nexus</span><span class="tool-role">tickets</span></div>
      <span class="flow-arrow" aria-hidden="true"></span>
      <div class="flow-node"><span class="tool-name">claude</span><span class="tool-role">agents</span></div>
      <span class="flow-arrow" aria-hidden="true"></span>
      <div class="flow-node"><span class="tool-name">xcode · git</span><span class="tool-role"><span class="gate-badge">gate</span></span></div>
      <span class="flow-arrow" aria-hidden="true"></span>
      <div class="flow-node"><span class="tool-name">review</span><span class="tool-role">every diff</span></div>
      <span class="flow-arrow" aria-hidden="true"></span>
      <div class="flow-node flow-placeholder"><span class="tool-name">local llms</span><span class="tool-role">in progress</span></div>
    </div>
    <p class="flow-caption">Multi-agent workflow, human-reviewed — I read every diff. <a href="/about/">more →</a></p>
  </section>

  <section id="contact">
    <h2>CONTACT</h2>
    <p>[<a href="https://www.linkedin.com/in/systemseven/" target="_blank" rel="noopener noreferrer">linkedin</a>]</p>
    <form class="contact-form" action="https://formspree.io/f/mvgyazvd" method="POST">
      <label for="email">your email</label>
      <input type="email" name="email" id="email" required autocomplete="email">
      <label for="message">your message</label>
      <textarea name="message" id="message" required rows="5"></textarea>
      <button type="submit">send &gt;</button>
    </form>
  </section>

</main>

<footer class="foot">
  <span>© 2026 · Ryan Saunders</span>
  <span class="jab">I added light mode. I'm not happy about it, but I did it.</span>
</footer>

</body>
</html>
```

Notes: the old smooth-scroll `<script>` is dropped (CSS `scroll-behavior: smooth` covers it). The `v26` footer span is dropped (placeholder copy Ryan may reinstate).

- [ ] **Step 2: Serve and verify**

Run (once, background, repo root): `python3 -m http.server 8891 --bind 127.0.0.1`
Run: `curl -s -o /dev/null -w "%{http_code}\n" http://127.0.0.1:8891/ http://127.0.0.1:8891/styles.css http://127.0.0.1:8891/images/icons/integenius-80.png`
Expected: three `200` lines.

- [ ] **Step 3: CHECKPOINT — Ryan previews http://localhost:8891 (homepage, both color schemes, narrow + wide)**

Wait for confirmation. Subpages will look broken at this point — say so explicitly when asking.

- [ ] **Step 4: Commit**

```bash
git add styles.css index.html
git commit -m "style(redesign): plaintext base system and homepage"
```

---

### Task 3: Subpage template chrome (crumb, title-block, prose, next-nav, callout, stack/pill, feature-list, meta-grid)

**Files:**
- Modify: `styles.css` (append section)
- Verify against: `edassists/index.html`, `cardinal/index.html`, `shoproute/index.html` (text-only pages, no markup edits expected)

- [ ] **Step 1: Append subpage template CSS to `styles.css`**

```css
/* ==========================================================================
   Subpage template (crumb / title-block / prose / next-nav)
   ========================================================================== */
.crumb { display: inline-block; color: var(--text-dim); margin-bottom: 2em; }
.crumb::before { content: "<- "; }

.title-block { margin-bottom: 1.5em; }
.title-block .eyebrow { color: var(--text-dim); font-size: 13px; }
.title-block h1 { text-transform: uppercase; overflow: hidden; }
.title-block h1::after {
  content: "================================================================================================================================================================================================";
  display: block; white-space: nowrap; color: var(--rule);
  font-weight: normal; letter-spacing: normal; margin-bottom: 0.4em;
}
.meta-grid { display: flex; flex-wrap: wrap; gap: 0.3ch 3ch; color: var(--text-dim); margin-top: 0.6em; }
.meta-grid .live::before, .meta-grid .running::before { content: "● "; }

.prose { max-width: 72ch; }
.prose p { margin: 0.9em 0; }
.prose ul { list-style: none; margin: 0.9em 0; }
.prose ul li::before { content: "* "; }
.prose ul li { margin: 0.35em 0; }

.callout { border: 1px dashed var(--rule); padding: 14px 18px; margin: 1.5em 0; color: var(--text-dim); }

.stack { display: flex; flex-wrap: wrap; gap: 8px; margin: 1em 0; }
.pill { border: 1px solid var(--rule); padding: 1px 10px; font-size: 13px; color: var(--text-dim); }

.feature-list { list-style: none; margin: 1em 0; }
.feature-list li { margin: 0.5em 0; }
.feature-list li::before { content: "* "; color: var(--text-dim); }

.next-nav {
  display: flex; justify-content: space-between; gap: 2ch; flex-wrap: wrap;
  margin-top: 3em; padding-top: 1em;
  border-top: 1px dashed var(--rule);
}
.next-nav a { color: var(--text-dim); }
```

- [ ] **Step 2: Verify the three text-only lab pages render correctly with NO markup edits**

Run: `curl -s -o /dev/null -w "%{http_code}\n" http://127.0.0.1:8891/edassists/ http://127.0.0.1:8891/cardinal/ http://127.0.0.1:8891/shoproute/`
Expected: three `200` lines. Then eyeball each in the browser (self-check before the Ryan checkpoint at Task 5).

- [ ] **Step 3: Grep the three pages for classes not yet styled**

Run: `grep -oh 'class="[^"]*"' edassists/index.html cardinal/index.html shoproute/index.html | tr ' ' '\n' | sort -u`
Compare against selectors in `styles.css`; add any missing class (using the vocabulary above — dim text, 1px borders, no fills) rather than leaving it unstyled.

- [ ] **Step 4: Commit**

```bash
git add styles.css
git commit -m "style(redesign): subpage template chrome"
```

---

### Task 4: Case-study pages with static hero images (integenius, solitairezero, sonny)

**Files:**
- Modify: `styles.css` (append)
- Modify (only if needed after inspection): `integenius/index.html`, `solitairezero/index.html`, `sonny/index.html`

- [ ] **Step 1: Append hero-image CSS**

```css
/* ---- Case-study hero image ---- */
.hero-image { margin: 1.6em 0 2em; }
.hero-image img, .hero-screenshot { display: block; width: 100%; height: auto; border: 1px solid var(--img-border); }
.hero-image figcaption { color: var(--text-dim); margin-top: 0.4em; font-size: 13px; }
```

- [ ] **Step 2: Check the three pages in the browser; strip any markup that fights the system**

Known candidates from inventory: `platform-icon` images in integenius's title block (keep, add `.platform-icon { height: 16px; vertical-align: text-bottom; }` if they render oversized). The `sz`/`nexus` variant classes on `hero-screenshot` were old-palette tints — they simply stop matching any rule; leave the markup alone.

- [ ] **Step 3: Commit**

```bash
git add styles.css integenius/index.html solitairezero/index.html sonny/index.html
git commit -m "style(redesign): case-study hero images"
```

(Include the HTML files in `git add` only if Step 2 actually modified them.)

---

### Task 5: Slider pages (kitchensynk, nexus) + module strip

**Files:**
- Modify: `styles.css` (append)
- Keep untouched: the inline slider `<script>` blocks in both pages

- [ ] **Step 1: Append reveal-slider + module-strip CSS**

The slider JS drives `--reveal` / `--hero-reveal-ratio` custom properties on existing class hooks (`hero-reveal`, `hero-reveal__layer`, `hero-reveal__top`, `hero-reveal__divider`, `hero-reveal__handle`, `hero-reveal__handle-icon`). Read the current rules for these classes in git history (`git show 82fe3b7:styles.css`, search `hero-reveal`) to preserve the functional properties (clip-path/width driven by the custom property, positioning, cursor, touch-action), then re-skin: divider becomes a 1px dashed `--rule` line, handle becomes a bordered `--bg` square with `<>` rendered in `--text` (replace any gradient/glow with flat tokens). Functional properties copied verbatim; only colors/borders/shadows change. No shadows, no radius.

```css
/* ---- Nexus module strip ---- */
.module-strip { display: grid; grid-template-columns: 1fr; gap: 16px; margin: 1.5em 0; }
@media (min-width: 720px) { .module-strip { grid-template-columns: repeat(5, 1fr); } }
.module-card { border: 1px solid var(--rule); padding: 10px 12px; }
.module-card .module-name { font-weight: bold; }
.module-card .module-desc { color: var(--text-dim); font-size: 13px; }
```

- [ ] **Step 2: Verify sliders still work**

In the browser: drag the reveal handle on /kitchensynk/ and /nexus/; keyboard arrows still move it. The slider images keep full color.

- [ ] **Step 3: Commit**

```bash
git add styles.css
git commit -m "style(redesign): reveal sliders and nexus module strip"
```

---

### Task 6: Neon Arcade page

**Files:**
- Modify: `styles.css` (append)

- [ ] **Step 1: Append arcade card CSS (reuses card vocabulary)**

```css
/* ---- Neon Arcade game cards ---- */
.arcade-games { display: grid; grid-template-columns: 1fr; gap: 16px; margin: 1.5em 0; }
@media (min-width: 720px) { .arcade-games { grid-template-columns: repeat(2, 1fr); } }
.arcade-game { border: 1px solid var(--rule); padding: 18px 18px 14px; }
.arcade-game-inspo { color: var(--text-dim); font-size: 13px; }
.arcade-game-title { font-weight: bold; }
.arcade-game-desc { color: var(--text-dim); }
.arcade-game-meta { margin-top: 1em; padding-top: 0.6em; border-top: 1px dashed var(--rule); color: var(--text-dim); font-size: 13px; }
```

- [ ] **Step 2: Eyeball /neon-arcade/ in browser; commit**

```bash
git add styles.css
git commit -m "style(redesign): neon arcade cards"
```

---

### Task 7: About page flow diagram

**Files:**
- Modify: `about/index.html`
- Modify: `styles.css` (only if the about markup can't map onto `.flow`)

- [ ] **Step 1: Convert the about-page `flow-wrap` markup to the homepage `.flow` vocabulary**

The about page has `flow-wrap` / `flow-node` / `flow-eyebrow` / `flow-role` / `gate-tag` markup (5 nodes, same pipeline as homepage). Rewrite that block to the exact homepage structure (`.flow` > `.flow-node`(`.tool-name`/`.tool-role`) + `.flow-arrow` spans + `.gate-badge` + `.flow-placeholder`), preserving the about page's node text verbatim (copy is placeholder but it's Ryan's to change, not ours). Also remove the `<span class="pulse">` from the about page's brand link.

- [ ] **Step 2: Eyeball /about/ in browser; commit**

```bash
git add about/index.html styles.css
git commit -m "style(redesign): about page flow diagram"
```

---

### Task 8: Privacy + contact boilerplate pages (CSS only)

**Files:**
- Modify: `styles.css` (append)
- No markup edits (6 pages: `{integenius,kitchensynk,solitairezero}/{privacy,contact}/index.html`)

- [ ] **Step 1: Append boilerplate-page CSS**

```css
/* ---- Privacy / contact boilerplate ---- */
.privacy-container, .contact-container { max-width: 72ch; margin: 0 auto; }
.privacy-content h1, .contact-content h1 { text-transform: uppercase; overflow: hidden; }
.privacy-content h1::after, .contact-content h1::after {
  content: "================================================================================================================================================================================================";
  display: block; white-space: nowrap; color: var(--rule);
  font-weight: normal; letter-spacing: normal; margin-bottom: 0.4em;
}
.privacy-content h2 { margin-top: 2em; }
.privacy-content p, .privacy-content ul { margin: 0.9em 0; color: var(--text); }
.privacy-content ul { list-style: none; }
.privacy-content ul li::before { content: "* "; }
.last-updated { color: var(--text-dim); font-size: 13px; }
.button-container { margin: 1.5em 0; }
.back-link { color: var(--text-dim); }
.back-link::before { content: "<- "; }
.form-group { margin-top: 1em; }
.form-group label { margin-top: 0; }
.submit-button {
  font-family: inherit; font-size: inherit;
  background: var(--bg); color: var(--text);
  border: 1px solid var(--text);
  padding: 6px 16px; margin-top: 1em; cursor: pointer;
}
.submit-button:hover { background: var(--text); color: var(--bg); }
```

- [ ] **Step 2: Verify all six pages**

Run: `for p in integenius kitchensynk solitairezero; do curl -s -o /dev/null -w "$p %{http_code}\n" http://127.0.0.1:8891/$p/privacy/; curl -s -o /dev/null -w "$p contact %{http_code}\n" http://127.0.0.1:8891/$p/contact/; done`
Expected: six `200`s; eyeball one privacy + one contact page in browser.

- [ ] **Step 3: Commit**

```bash
git add styles.css
git commit -m "style(redesign): privacy and contact boilerplate pages"
```

---

### Task 9: Sweep, dead CSS, lcars decision, final checkpoint

**Files:**
- Modify: `styles.css` (prune), possibly small HTML fixes

- [ ] **Step 1: Orphan-class sweep across all pages**

Run: `grep -rhoE 'class="[^"]*"' --include='*.html' . --exclude-dir=.git --exclude-dir=.superpowers --exclude-dir=.sonny --exclude-dir=sandbox --exclude-dir=docs | tr ' ' '\n' | sed 's/class="//;s/"//' | sort -u`
Every class in the output must either match a selector in `styles.css` or be intentionally unstyled (variant tints like `sz`, `kt`, `integenius`, `nexus` on tiles/screenshots). Fix stragglers with the established vocabulary.

- [ ] **Step 2: Dead-selector prune**

For each class selector in the new `styles.css`, confirm at least one HTML page references it; delete selectors that nothing references. (The old 1437-line sheet is fully replaced, so this should be quick.)

- [ ] **Step 3: lcars page decision**

`lcars/index.html` was excluded from inventory. Inspect it: if it carries its own self-contained theme (LCARS homage) and does not link `/styles.css`, leave it untouched and note that in the final summary. If it consumes `/styles.css`, restyle with the same vocabulary.

- [ ] **Step 4: FINAL CHECKPOINT — Ryan walks the whole site at http://localhost:8891**

Both color schemes, mobile width, every nav path. Wait for approval.

- [ ] **Step 5: Final commit**

```bash
git add -A
git commit -m "style(redesign): plaintext sweep and dead CSS prune"
```

Remind Ryan at the end: nothing has been pushed; pushing `main` deploys the live site, so the push is his call, as is the copy rewrite pass.
