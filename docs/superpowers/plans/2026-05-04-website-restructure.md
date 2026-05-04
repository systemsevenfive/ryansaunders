# Website Restructure Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a Tools section (Nexus + Sonny), a Neon Arcade umbrella page, and a new Nexus project page to ryansaunders.ca, while reordering homepage sections and updating nav across the site.

**Architecture:** Pure static HTML/CSS site — no build step, no framework. All changes are direct edits to HTML files and `styles.css`. New pages follow the exact structural pattern of existing project pages (see `integenius/index.html` as the reference template for shipped/tools pages).

**Tech Stack:** HTML5, CSS custom properties, no JS required for any new feature.

**Spec:** `docs/superpowers/specs/2026-05-04-website-restructure-design.md`
**Design references (Nexus Designs tab):** `2026-05-04-homepage-restructure-v2-updated-copy`, `2026-05-04-nexus-page-structure-draft`, `2026-05-04-neon-arcade-page-layout-options`

---

## File Map

| File | Action | What changes |
|------|--------|-------------|
| `styles.css` | Modify | Add `--accent-violet` tokens, `.tools` grid, `.tool-tile`, `.module-strip`, `.module-card`, `.lab-tile.neon-arcade`, `.hero-image.nexus`, `.meta .tools-count b`, `.title-block .meta-grid .running` |
| `index.html` | Modify | Nav (add Tools), hero counter (add Tools chip), new Tools section, Lab renumbered to 03, Sonny tile removed, Neon Arcade tile added, About → 04, Contact → 05 |
| `nexus/index.html` | Create | New Tools project page |
| `neon-arcade/index.html` | Create | New Lab umbrella page |
| `sonny/index.html` | Rewrite | Eyebrow, tag, meta-grid, all prose, breadcrumb, next-nav |
| `edassists/index.html` | Modify | Prev-nav: Sonny → Chartula; nav: add Tools link |
| `chartula/index.html` | Modify | Next-nav: Sonny → Ed Assists; nav: add Tools link |
| `integenius/index.html` | Modify | Nav: add Tools link |
| `kitchentime/index.html` | Modify | Nav: add Tools link |
| `solitairezero/index.html` | Modify | Nav: add Tools link |
| `shoproute/index.html` | Modify | Nav: add Tools link |
| `lcars/index.html` | Modify | Nav: add Tools link |
| `about/index.html` | Modify | Nav: add Tools link |

Subpages (privacy/, contact/ inside each project) may also have the nav — check and update any that do.

---

## Task 1: CSS — Violet tokens, tool-tile, module-strip, neon-arcade variant

**Files:**
- Modify: `styles.css`

- [ ] **Step 1: Add `--accent-violet` tokens to `:root` (dark mode)**

In `styles.css`, find the block ending with `--accent-cyan-soft: rgba(0, 200, 255, 0.14);` inside `:root` and add immediately after:

```css
  --accent-violet:      #a78bfa;
  --accent-violet-soft: rgba(167, 139, 250, 0.14);
```

- [ ] **Step 2: Add light-mode violet tokens**

Find the `@media (prefers-color-scheme: light)` block inside `:root` and add after the last `--accent-cyan` line:

```css
    --accent-violet:      #7c3aed;
    --accent-violet-soft: rgba(124, 58, 237, 0.10);
```

- [ ] **Step 3: Add `.meta .tools-count b` rule and `.title-block .meta-grid .running`**

Find `.hero .meta b { color: var(--text); font-weight: 600; }` and add immediately after:

```css
.hero .meta .tools-count b { color: var(--accent-violet); }
```

Find `.title-block .meta-grid .live { color: #30d158; }` and add immediately after:

```css
.title-block .meta-grid .running { color: var(--accent-violet); }
```

Also add to the light-mode media query block where `.live` and `.soon` are overridden:

```css
  .title-block .meta-grid .running { color: #6d28d9; }
```

- [ ] **Step 4: Add `.hero-image.nexus` gradient**

Find the line `.hero-image.sz { background: ... }` and add immediately after:

```css
.hero-image.nexus { background: linear-gradient(135deg, #ddd6fe 0%, #a78bfa 60%, #4c1d95 140%); }
```

- [ ] **Step 5: Add `.tools` grid and `.tool-tile` styles**

Find the comment `/* ========== Lab grid ========== */` and insert the following block immediately before it:

```css
/* ========== Tools grid ========== */

.tools {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
  padding-bottom: 20px;
}
@media (max-width: 900px) { .tools { grid-template-columns: 1fr; } }

.tool-tile {
  display: block;
  border: 1px solid var(--rule-strong);
  border-radius: var(--r-md);
  padding: 26px 22px;
  background: var(--tile-bg);
  text-decoration: none;
  color: inherit;
  position: relative;
  overflow: hidden;
  transition: border-color 0.25s ease, transform 0.25s ease;
}
.tool-tile::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 2px;
  background: linear-gradient(90deg, var(--accent-violet), transparent);
}
.tool-tile:hover,
.tool-tile:focus-visible {
  border-color: var(--accent-violet);
  transform: translateY(-2px);
}

.tool-tile .monogram {
  width: 40px;
  height: 40px;
  border-radius: var(--r-md);
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: var(--font-display);
  font-size: 16px;
  font-weight: 700;
  background: var(--accent-violet-soft);
  border: 1px solid rgba(167, 139, 250, 0.25);
  color: var(--accent-violet);
}

.tool-tile h3 {
  font-family: var(--font-display);
  font-size: 19px;
  font-weight: 700;
  letter-spacing: -0.02em;
  margin: 0 0 6px;
  color: var(--text);
}
.tool-tile .sub {
  font-size: 13px;
  color: var(--text-muted);
  margin: 0 0 14px;
  line-height: 1.4;
}
.tool-tile .module-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin: 0 0 16px;
}
.tool-tile .module-tag {
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  border: 1px solid rgba(167, 139, 250, 0.25);
  color: var(--accent-violet);
  background: var(--accent-violet-soft);
  border-radius: var(--r-sm);
  padding: 2px 6px;
}
.tool-tile .integration-note {
  font-family: var(--font-mono);
  font-size: 11px;
  color: var(--text-faint);
  margin: 0 0 16px;
  line-height: 1.5;
}
.tool-tile .tile-foot {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text-faint);
  border-top: 1px solid var(--rule);
  padding-top: 14px;
  margin-top: 4px;
}
.tool-tile .status-running { color: var(--accent-violet); }
```

- [ ] **Step 6: Add `.module-strip` and `.module-card` styles (for the Nexus page)**

Find the comment `/* ========== About teaser ========== */` and insert immediately before it:

```css
/* ========== Module strip (Nexus page) ========== */

.module-strip {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 12px;
  margin-bottom: 36px;
}
@media (max-width: 900px) { .module-strip { grid-template-columns: repeat(3, 1fr); } }
@media (max-width: 560px) { .module-strip { grid-template-columns: repeat(2, 1fr); } }

.module-card {
  border: 1px solid rgba(167, 139, 250, 0.25);
  background: var(--accent-violet-soft);
  border-radius: var(--r-md);
  padding: 16px 14px;
}
.module-card .module-name {
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--accent-violet);
  margin-bottom: 6px;
  font-weight: 600;
}
.module-card .module-desc {
  font-size: 12px;
  color: var(--text-muted);
  line-height: 1.4;
  margin: 0;
}
```

- [ ] **Step 7: Add `.lab-tile.neon-arcade` variant**

Find `.lab-tile .bottom {` block, scroll to the end of its closing `}`, and add immediately after:

```css
.lab-tile.neon-arcade {
  border-color: rgba(255, 45, 149, 0.35);
  background: rgba(255, 45, 149, 0.03);
}
.lab-tile.neon-arcade .label { color: var(--accent); }
.lab-tile.neon-arcade .bottom { color: var(--accent); opacity: 0.7; }
```

- [ ] **Step 8: Verify CSS additions with grep**

Run:
```bash
grep -n "accent-violet\|tool-tile\|module-strip\|neon-arcade" /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/styles.css
```

Expected: lines for each new rule, no errors.

- [ ] **Step 9: Commit**

```bash
git -C /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders add styles.css
git -C /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders commit -m "feat(styles): add violet tokens, tool-tile, module-strip, neon-arcade variant [RYAN]"
```

---

## Task 2: Homepage restructure

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add Tools nav link**

Find:
```html
    <a href="#shipped" aria-current="page">Work</a>
    <a href="#lab">Lab</a>
```
Replace with:
```html
    <a href="#shipped" aria-current="page">Work</a>
    <a href="#tools">Tools</a>
    <a href="#lab">Lab</a>
```

- [ ] **Step 2: Add Tools chip to hero meta counters**

Find:
```html
      <span><b>3</b> Shipped</span>
      <span><b>5</b> In the lab</span>
```
Replace with:
```html
      <span><b>3</b> Shipped</span>
      <span class="tools-count"><b>2</b> Tools</span>
      <span><b>5</b> In the lab</span>
```

- [ ] **Step 3: Add Tools section after the Shipped section**

Find the line:
```html
  <!-- Lab -->
  <header class="sec-head">
    <h2><span class="num">02</span>In The lab</h2>
```
Insert the following block immediately before it:

```html
  <!-- Tools -->
  <header class="sec-head" id="tools">
    <h2><span class="num">02</span>Tools</h2>
    <span class="sec-note">Built &amp; running</span>
  </header>
  <section class="tools">
    <a class="tool-tile" href="/nexus/">
      <div class="monogram">N</div>
      <h3>Nexus</h3>
      <p class="sub">My personal dev workflow hub. Built because the alternatives were either too much or too little.</p>
      <div class="module-tags">
        <span class="module-tag">Tickets</span>
        <span class="module-tag">Reports</span>
        <span class="module-tag">Designs</span>
        <span class="module-tag">Architecture</span>
        <span class="module-tag">Glossary</span>
      </div>
      <div class="tile-foot"><span class="status-running">● Running</span><span>Local · Node + Web</span></div>
    </a>
    <a class="tool-tile" href="/sonny/">
      <div class="monogram">S</div>
      <h3>Sonny</h3>
      <p class="sub">Analyzes a Swift codebase and maps its architecture as a navigable graph — types, dependencies, and relationships at a glance.</p>
      <p class="integration-note">Also available as a standalone package — integrated into Nexus for cross-project views.</p>
      <div class="tile-foot"><span class="status-running">● Running</span><span>Swift · npm package</span></div>
    </a>
  </section>

```

- [ ] **Step 4: Renumber Lab to 03, add `id="lab"`, remove Sonny tile, add Neon Arcade tile**

Find the entire Lab section:
```html
  <!-- Lab -->
  <header class="sec-head">
    <h2><span class="num">02</span>In The lab</h2>
    <span class="sec-note">In progress · prototypes</span>
  </header>
  <section class="lab" id="lab">
    <a class="lab-tile" href="/edassists/">
      <div class="label">EXP-001 · TOOL</div>
      <h3>Ed Assists</h3>
      <p class="sub">Ed would like to Assist the video editors of the world. </p>
      <div class="bottom">PROTOTYPE</div>
    </a>
    <a class="lab-tile" href="/shoproute/">
      <div class="label">EXP-002 · ML</div>
      <h3>ShopRoute</h3>
      <p class="sub">Many people shop at just a few stores in a small radius from their home. Can we make their shopping more cost-effective?</p>
      <div class="bottom">PROTOTYPE</div>
    </a>
    <a class="lab-tile" href="/lcars/">
      <div class="label">EXP-003 · UI</div>
      <h3>LCARS</h3>
      <p class="sub">A tribute to Star Trek: The Next Generation, this is my personal Home app alternative combined with news and weather.</p>
      <div class="bottom">TINKERING</div>
    </a>
    <a class="lab-tile" href="/chartula/">
      <div class="label">EXP-004 · TOOL</div>
      <h3>Chartula</h3>
      <p class="sub">Keep those business cards organized, and integrated with maps.</p>
      <div class="bottom">TINKERING</div>
    </a>
    <a class="lab-tile" href="/sonny/">
      <div class="label">EXP-005 · AI</div>
      <h3>Sonny</h3>
      <p class="sub">A navigable graph of software concepts. Today, a personal tech-term glossary I keep fresh. Tomorrow, a way to get up to speed in unfamiliar iOS codebases.</p>
      <div class="bottom">TINKERING</div>
    </a>
  </section>
```

Replace with:

```html
  <!-- Lab -->
  <header class="sec-head">
    <h2><span class="num">03</span>In The Lab</h2>
    <span class="sec-note">In progress · prototypes</span>
  </header>
  <section class="lab" id="lab">
    <a class="lab-tile" href="/edassists/">
      <div class="label">EXP-001 · TOOL</div>
      <h3>Ed Assists</h3>
      <p class="sub">Ed would like to Assist the video editors of the world.</p>
      <div class="bottom">PROTOTYPE</div>
    </a>
    <a class="lab-tile" href="/shoproute/">
      <div class="label">EXP-002 · ML</div>
      <h3>ShopRoute</h3>
      <p class="sub">Many people shop at just a few stores in a small radius from their home. Can we make their shopping more cost-effective?</p>
      <div class="bottom">PROTOTYPE</div>
    </a>
    <a class="lab-tile" href="/lcars/">
      <div class="label">EXP-003 · UI</div>
      <h3>LCARS</h3>
      <p class="sub">A tribute to Star Trek: The Next Generation, this is my personal Home app alternative combined with news and weather.</p>
      <div class="bottom">TINKERING</div>
    </a>
    <a class="lab-tile" href="/chartula/">
      <div class="label">EXP-004 · TOOL</div>
      <h3>Chartula</h3>
      <p class="sub">Keep those business cards organized, and integrated with maps.</p>
      <div class="bottom">TINKERING</div>
    </a>
    <a class="lab-tile neon-arcade" href="/neon-arcade/">
      <div class="label">ARCADE · GAMES</div>
      <h3>The Neon Arcade</h3>
      <p class="sub">Classics rebuilt for iPad. No IAP, no timers, no dark patterns. Just the game.</p>
      <div class="bottom">IN DEVELOPMENT</div>
    </a>
  </section>
```

- [ ] **Step 5: Renumber About to 04**

Find:
```html
  <header class="sec-head">
    <h2><span class="num">03</span>About</h2>
  </header>
```
Replace with:
```html
  <header class="sec-head">
    <h2><span class="num">04</span>About</h2>
  </header>
```

- [ ] **Step 6: Renumber Contact to 05**

Find:
```html
  <header class="sec-head" id="contact">
    <h2><span class="num">04</span>Contact</h2>
  </header>
```
Replace with:
```html
  <header class="sec-head" id="contact">
    <h2><span class="num">05</span>Contact</h2>
  </header>
```

- [ ] **Step 7: Verify key elements exist**

```bash
grep -n "id=\"tools\"\|tools-count\|neon-arcade\|num.*03.*In The Lab\|num.*04.*About\|num.*05.*Contact" /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/index.html
```

Expected: one match for each pattern.

- [ ] **Step 8: Commit**

```bash
git -C /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders add index.html
git -C /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders commit -m "feat(home): add Tools section, Neon Arcade tile, reorder sections [RYAN]"
```

---

## Task 3: New Nexus page

**Files:**
- Create: `nexus/index.html`

- [ ] **Step 1: Create the directory and file**

```bash
mkdir -p /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/nexus
```

- [ ] **Step 2: Write `nexus/index.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nexus — Ryan Saunders</title>
  <meta name="description" content="Nexus — my personal dev workflow hub. Tickets, reports, designs, architecture, and knowledge base in one local interface.">
  <link rel="stylesheet" href="/styles.css">
</head>
<body>

<nav class="nav" aria-label="Primary">
  <a href="/" class="brand"><span class="pulse" aria-hidden="true"></span>RYAN SAUNDERS</a>
  <div class="links">
    <a href="/#shipped">Work</a>
    <a href="/#tools" aria-current="page">Tools</a>
    <a href="/#lab">Lab</a>
    <a href="/about/">About</a>
    <a href="/#contact">Contact</a>
  </div>
</nav>

<main class="page">

  <div class="crumb">← <a href="/#tools">Back to Tools</a></div>

  <article>
    <header class="title-block">
      <div class="eyebrow">Tools · Built &amp; Running</div>
      <h1>Nexus</h1>
      <p class="tag">My personal dev workflow hub. Tickets, reports, designs, architecture, and knowledge base — one interface, built to fit how I actually work.</p>
      <div class="meta-grid">
        <div>Status<b class="running">● Running</b></div>
        <div>Year<b>2025 — 2026</b></div>
        <div>Platform<b>Node.js · Web</b></div>
        <div>Repo<b>Local · Private</b></div>
      </div>
    </header>

    <div class="module-strip">
      <div class="module-card">
        <div class="module-name">Tickets</div>
        <p class="module-desc">Lightweight board and ticket system across all projects</p>
      </div>
      <div class="module-card">
        <div class="module-name">Reports</div>
        <p class="module-desc">Themed viewer for Claude session outputs — standups, post-mortems, insights</p>
      </div>
      <div class="module-card">
        <div class="module-name">Designs</div>
        <p class="module-desc">Living record of design decisions and Claude-authored mockups</p>
      </div>
      <div class="module-card">
        <div class="module-name">Architecture</div>
        <p class="module-desc">Sonny-powered graph views of every app project</p>
      </div>
      <div class="module-card">
        <div class="module-name">Glossary</div>
        <p class="module-desc">A navigable knowledge base of concepts and relationships</p>
      </div>
    </div>

    <div class="hero-image nexus" aria-hidden="true"></div>

    <section class="prose">
      <h2><span class="num">01</span>Why it exists</h2>
      <p>I was using Jira for personal project tracking. That sentence should explain itself. At the other extreme, a plain text file doesn't survive a multi-agent session where several things are moving at once. I needed something in between — something that understood my workflow, not the other way around.</p>
      <p>What started as a simple ticket board grew into a full workflow hub as each gap became obvious. Reports needed a home. Design decisions needed a record. Architecture needed to be visible without opening Xcode. Eventually I had five tools in one interface, all talking to each other.</p>

      <h2><span class="num">02</span>What I built</h2>
      <p>Nexus runs locally as a Node.js server backed by plain JSON files. The ticket system handles multiple project spaces, full state transitions, comments, branches, and PR links — with auto-commits to a separate data repo so the code stays clean.</p>
      <p>The reports hub is a themed viewer for everything Claude generates during a session: standups, post-mortems, swarm summaries, insights. Themes are first-class — a theme added today applies to every report ever written. The designs tab is where mockups and visual decisions live, so they don't get lost in chat history.</p>
      <p>The architecture tab runs Sonny against any project and renders the result as a navigable graph — types, dependencies, relationships — without leaving the hub. The glossary is the same graph format, keeping concepts and their relationships explorable rather than buried in a document.</p>

      <div class="callout">"The best tool is the one that disappears. Nexus isn't interesting — my projects are. It just tries to stay out of the way."</div>

      <div class="stack">
        <span class="pill">Node.js</span>
        <span class="pill">Fastify</span>
        <span class="pill">Vanilla JS</span>
        <span class="pill">Sonny</span>
        <span class="pill">JSON</span>
        <span class="pill">Git</span>
      </div>

      <h2><span class="num">03</span>What's next</h2>
      <p>Nexus evolves with my workflow. Each agent session that surfaces a gap becomes a ticket. It's a tool I use every day, which means it gets better every day.</p>
    </section>
  </article>

  <nav class="next-nav" aria-label="Tools navigation">
    <a href="/sonny/">
      <div class="label">Next →</div>
      <div class="title">Sonny</div>
    </a>
  </nav>

</main>

<footer class="foot">
  <span>© 2026 · RYAN SAUNDERS</span>
  <span class="jab">You're viewing this in light mode — bold choice.</span>
  <span>v26</span>
</footer>

</body>
</html>
```

Note: the `.hero-image.nexus` div shows the violet gradient placeholder. When Ryan has taken the screenshot, replace the div with:
```html
    <div class="hero-image nexus">
      <img class="hero-screenshot" src="/nexus/images/nexus-reports.png" alt="The Nexus reports hub in the Aqua theme, showing report types in the sidebar and recent session reports in the main panel.">
    </div>
```
And create the `nexus/images/` directory for the screenshot file.

- [ ] **Step 3: Verify the file**

```bash
grep -n "module-strip\|module-card\|hero-image nexus\|Back to Tools" /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/nexus/index.html
```

Expected: matches for each pattern.

- [ ] **Step 4: Commit**

```bash
git -C /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders add nexus/index.html
git -C /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders commit -m "feat(nexus): add Nexus project page [RYAN]"
```

---

## Task 4: New Neon Arcade page

**Files:**
- Create: `neon-arcade/index.html`

- [ ] **Step 1: Create the directory**

```bash
mkdir -p /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/neon-arcade
```

- [ ] **Step 2: Write `neon-arcade/index.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>The Neon Arcade — Ryan Saunders</title>
  <meta name="description" content="The Neon Arcade — classic arcade games rebuilt for iPad. No IAP, no dark patterns. A lab project by Ryan Saunders.">
  <link rel="stylesheet" href="/styles.css">
</head>
<body>

<nav class="nav" aria-label="Primary">
  <a href="/" class="brand"><span class="pulse" aria-hidden="true"></span>RYAN SAUNDERS</a>
  <div class="links">
    <a href="/#shipped">Work</a>
    <a href="/#tools">Tools</a>
    <a href="/#lab" aria-current="page">Lab</a>
    <a href="/about/">About</a>
    <a href="/#contact">Contact</a>
  </div>
</nav>

<main class="page">

  <div class="crumb">← <a href="/#lab">Back to Lab</a></div>

  <article>
    <header class="title-block">
      <div class="eyebrow">Lab · In Development</div>
      <h1>The Neon Arcade</h1>
      <p class="tag">Classics rebuilt for iPad. No IAP, no energy timers, no dark patterns. Just the game.</p>
      <div class="meta-grid">
        <div>Status<b>● In Development</b></div>
        <div>Platform<b>iOS 26 · iPadOS 26</b></div>
        <div>Engine<b>SpriteKit</b></div>
        <div>Titles<b>2 in progress</b></div>
      </div>
    </header>

    <section class="prose">
      <h2><span class="num">01</span>Why it exists</h2>
      <p>I grew up on Ambrosia Software games. Not the originals they were inspired by — the Mac shareware versions, with registered serial numbers and a $25 cheque in the mail. That era of game design had a clarity modern mobile gaming has mostly abandoned: here's the game, here are the rules, get better at it.</p>
      <p>The Neon Arcade is my attempt to bring that back for iPad. Familiar arcade mechanics, rebuilt from scratch with native Apple frameworks, optimized for touch. No Unity, no IAP, no subscriptions. Just the game.</p>

      <h2><span class="num">02</span>The games</h2>

      <div class="arcade-games">
        <div class="arcade-game">
          <div class="arcade-game-inspo">Inspired by Centipede / Apeiron</div>
          <h3 class="arcade-game-title">Scolonoids</h3>
          <p class="arcade-game-desc">A neon biomechanical Centipede. You're a gardener drone pruning a derelict living hive. Roguelike runs, charge beam as the central feel verb, offset-drag touch input.</p>
          <div class="arcade-game-meta">
            <span>IN DEVELOPMENT</span>
            <span>iOS 26 · SpriteKit</span>
          </div>
        </div>
        <div class="arcade-game">
          <div class="arcade-game-inspo">Inspired by Harry the Handsome Executive</div>
          <h3 class="arcade-game-title">Circle Back And Destroy</h3>
          <p class="arcade-game-desc">Top-down multi-floor escape game. Navigate hostile office environments, hijack ally robots, and get out. Built entirely on native Apple frameworks — no Unity, no third-party engines.</p>
          <div class="arcade-game-meta">
            <span>EARLY DEV</span>
            <span>iOS 26 · SpriteKit</span>
          </div>
        </div>
      </div>

      <div class="stack">
        <span class="pill">SpriteKit</span>
        <span class="pill">SwiftUI</span>
        <span class="pill">GameplayKit</span>
        <span class="pill">GameController</span>
        <span class="pill">Swift 6</span>
        <span class="pill">iOS 26</span>
      </div>

      <h2><span class="num">03</span>What's next</h2>
      <p>Scolonoids is further along — the core arcade loop, roguelike run structure, and touch input are all working. CBAD is at the proof-of-concept engine stage. Both are built with the same philosophy: native frameworks only, no shortcuts.</p>
    </section>
  </article>

</main>

<footer class="foot">
  <span>© 2026 · RYAN SAUNDERS</span>
  <span class="jab">You're viewing this in light mode — bold choice.</span>
  <span>v26</span>
</footer>

</body>
</html>
```

- [ ] **Step 3: Add `.arcade-games` CSS to `styles.css`**

Find the `/* ========== About teaser ========== */` comment and insert immediately before it:

```css
/* ========== Arcade games list (Neon Arcade page) ========== */

.arcade-games {
  display: flex;
  flex-direction: column;
  gap: 0;
  margin: 20px 0 28px;
  max-width: var(--max-prose);
}
.arcade-game {
  border: 1px solid var(--rule-strong);
  border-radius: var(--r-md);
  padding: 22px 24px;
  background: var(--tile-bg);
  margin-bottom: 12px;
  display: grid;
  grid-template-columns: 1fr auto;
  grid-template-rows: auto auto auto;
  gap: 0 20px;
}
.arcade-game-inspo {
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--accent-cyan);
  margin-bottom: 8px;
  grid-column: 1;
}
.arcade-game-title {
  font-family: var(--font-display);
  font-size: 20px;
  font-weight: 700;
  letter-spacing: -0.02em;
  margin: 0 0 10px;
  color: var(--text);
  grid-column: 1;
}
.arcade-game-desc {
  font-size: 14px;
  color: var(--text-muted);
  line-height: 1.5;
  margin: 0;
  grid-column: 1;
}
.arcade-game-meta {
  grid-column: 2;
  grid-row: 1 / 4;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 4px;
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--text-faint);
  white-space: nowrap;
  padding-top: 2px;
}
@media (max-width: 640px) {
  .arcade-game { grid-template-columns: 1fr; }
  .arcade-game-meta { grid-column: 1; grid-row: auto; flex-direction: row; gap: 12px; margin-top: 14px; }
}
```

- [ ] **Step 4: Verify**

```bash
grep -n "arcade-game\|Scolonoids\|Circle Back" /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/neon-arcade/index.html
grep -n "arcade-game" /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/styles.css
```

Expected: matches in both files.

- [ ] **Step 5: Commit**

```bash
git -C /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders add neon-arcade/index.html styles.css
git -C /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders commit -m "feat(neon-arcade): add Neon Arcade umbrella page and arcade-game styles [RYAN]"
```

---

## Task 5: Rewrite Sonny page

**Files:**
- Modify: `sonny/index.html`

- [ ] **Step 1: Replace `sonny/index.html` with updated content**

Write the complete file:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sonny — Ryan Saunders</title>
  <meta name="description" content="Sonny — Swift codebase architecture analyzer. A tool by Ryan Saunders.">
  <link rel="stylesheet" href="/styles.css">
</head>
<body>

<nav class="nav" aria-label="Primary">
  <a href="/" class="brand"><span class="pulse" aria-hidden="true"></span>RYAN SAUNDERS</a>
  <div class="links">
    <a href="/#shipped">Work</a>
    <a href="/#tools" aria-current="page">Tools</a>
    <a href="/#lab">Lab</a>
    <a href="/about/">About</a>
    <a href="/#contact">Contact</a>
  </div>
</nav>

<main class="page">

  <div class="crumb">← <a href="/#tools">Back to Tools</a></div>

  <article>
    <header class="title-block">
      <div class="eyebrow">Tools · Running</div>
      <h1>Sonny</h1>
      <p class="tag">Analyzes a Swift codebase and maps its architecture as a navigable graph.</p>
      <div class="meta-grid">
        <div>Status<b class="running">● Running</b></div>
        <div>Year<b>2026</b></div>
        <div>Platform<b>Swift · npm package</b></div>
        <div>Integration<b>Nexus · Standalone</b></div>
      </div>
    </header>

    <section class="prose">
      <h2><span class="num">01</span>Why it exists</h2>
      <p>Ramping up on an unfamiliar iOS codebase takes time. Not because the code is necessarily hard — because understanding how its parts relate takes reading, and reading takes time. I wanted a faster entry point: something that could walk a project and show me its structure as a map instead of a file tree.</p>
      <p>The same problem applies to my own projects. With several apps in active development at once, having an at-a-glance view of each one's architecture — types, dependencies, view-model relationships — is more useful than opening Xcode every time.</p>

      <h2><span class="num">02</span>What it does</h2>
      <p>Point Sonny at a Swift project and it walks the source tree, identifies the major structural elements — types, protocols, view models, services, dependencies — and emits a versioned JSON model describing the graph. The renderer takes that model and draws it as a navigable, zoomable graph you can explore in a browser.</p>
      <p>The contract between analyzer and renderer is a small, stable JSON schema. That separation means the renderer works the same whether the model was generated by Sonny, written by hand, or produced by a future tool I haven't built yet.</p>

      <div class="callout">A clean contract beats a clever framework. Make the data shape obvious and the rest gets easier.</div>

      <div class="stack">
        <span class="pill">Swift</span>
        <span class="pill">Cytoscape.js</span>
        <span class="pill">JSON Schema</span>
        <span class="pill">npm package</span>
        <span class="pill">Vanilla JS</span>
      </div>

      <h2><span class="num">03</span>What's next</h2>
      <p>Sonny is integrated into Nexus as the architecture tab — run it against any project in the hub and explore the graph without leaving your workflow. It's also available as a standalone npm package for use outside Nexus, which means it can travel to work with me.</p>
    </section>
  </article>

  <nav class="next-nav" aria-label="Tools navigation">
    <a href="/nexus/">
      <div class="label">← Previous</div>
      <div class="title">Nexus</div>
    </a>
  </nav>

</main>

<footer class="foot">
  <span>© 2026 · RYAN SAUNDERS</span>
  <span class="jab">You're viewing this in light mode — bold choice.</span>
  <span>v26</span>
</footer>

</body>
</html>
```

- [ ] **Step 2: Verify**

```bash
grep -n "Back to Tools\|Tools · Running\|class=\"running\"\|npm package" /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/sonny/index.html
```

Expected: matches for each pattern.

- [ ] **Step 3: Commit**

```bash
git -C /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders add sonny/index.html
git -C /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders commit -m "feat(sonny): rewrite page for architecture-analysis focus, move to Tools [RYAN]"
```

---

## Task 6: Repair Lab next-nav chain and update nav in edassists + chartula

**Files:**
- Modify: `edassists/index.html`
- Modify: `chartula/index.html`

- [ ] **Step 1: Fix Ed Assists prev-nav (was: ← Sonny, should be: ← Chartula)**

In `edassists/index.html`, find:
```html
  <nav class="next-nav" aria-label="Lab navigation">
    <a href="/sonny/">
      <div class="label">← Previous</div>
      <div class="title">Sonny</div>
    </a>
    <a href="/shoproute/">
      <div class="label">Next →</div>
      <div class="title">ShopRoute</div>
    </a>
  </nav>
```
Replace with:
```html
  <nav class="next-nav" aria-label="Lab navigation">
    <a href="/chartula/">
      <div class="label">← Previous</div>
      <div class="title">Chartula</div>
    </a>
    <a href="/shoproute/">
      <div class="label">Next →</div>
      <div class="title">ShopRoute</div>
    </a>
  </nav>
```

- [ ] **Step 2: Add Tools link to Ed Assists nav**

In `edassists/index.html`, find:
```html
    <a href="/#shipped">Work</a>
    <a href="/#lab" aria-current="page">Lab</a>
```
Replace with:
```html
    <a href="/#shipped">Work</a>
    <a href="/#tools">Tools</a>
    <a href="/#lab" aria-current="page">Lab</a>
```

- [ ] **Step 3: Fix Chartula next-nav (was: → Sonny, should be: → Ed Assists)**

In `chartula/index.html`, find:
```html
  <nav class="next-nav" aria-label="Lab navigation">
    <a href="/lcars/">
      <div class="label">← Previous</div>
      <div class="title">LCARS</div>
    </a>
    <a href="/sonny/">
      <div class="label">Next →</div>
      <div class="title">Sonny</div>
    </a>
  </nav>
```
Replace with:
```html
  <nav class="next-nav" aria-label="Lab navigation">
    <a href="/lcars/">
      <div class="label">← Previous</div>
      <div class="title">LCARS</div>
    </a>
    <a href="/edassists/">
      <div class="label">Next →</div>
      <div class="title">Ed Assists</div>
    </a>
  </nav>
```

- [ ] **Step 4: Add Tools link to Chartula nav**

In `chartula/index.html`, find:
```html
    <a href="/#shipped">Work</a>
    <a href="/#lab" aria-current="page">Lab</a>
```
Replace with:
```html
    <a href="/#shipped">Work</a>
    <a href="/#tools">Tools</a>
    <a href="/#lab" aria-current="page">Lab</a>
```

- [ ] **Step 5: Verify both files**

```bash
grep -n "Chartula\|ShopRoute" /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/edassists/index.html
grep -n "Ed Assists\|LCARS" /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/chartula/index.html
grep -n "/#tools" /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/edassists/index.html
grep -n "/#tools" /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/chartula/index.html
```

Expected: Chartula + ShopRoute in edassists; Ed Assists + LCARS in chartula; `/#tools` in both.

- [ ] **Step 6: Commit**

```bash
git -C /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders add edassists/index.html chartula/index.html
git -C /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders commit -m "fix(lab): repair next-nav chain after Sonny moves to Tools, add Tools nav link [RYAN]"
```

---

## Task 7: Add Tools nav link to all remaining pages

**Files:**
- Modify: `integenius/index.html`
- Modify: `kitchentime/index.html`
- Modify: `solitairezero/index.html`
- Modify: `shoproute/index.html`
- Modify: `lcars/index.html`
- Modify: `about/index.html`
- Check and update any `privacy/` and `contact/` subpages that include the nav

The change is identical in every file. Find:
```html
    <a href="/#shipped" aria-current="page">Work</a>
    <a href="/#lab">Lab</a>
```
or (for lab pages):
```html
    <a href="/#shipped">Work</a>
    <a href="/#lab" aria-current="page">Lab</a>
```
or (for about):
```html
    <a href="/#shipped">Work</a>
    <a href="/#lab">Lab</a>
    <a href="/about/" aria-current="page">About</a>
```

In every case, add `<a href="/#tools">Tools</a>` between `Work` and `Lab`.

- [ ] **Step 1: Update `integenius/index.html` nav**

Find `<a href="/#shipped" aria-current="page">Work</a>` and the `Lab` link immediately after it, add Tools between them:
```html
    <a href="/#shipped" aria-current="page">Work</a>
    <a href="/#tools">Tools</a>
    <a href="/#lab">Lab</a>
```

- [ ] **Step 2: Update `kitchentime/index.html` nav** — same pattern as Step 1.

- [ ] **Step 3: Update `solitairezero/index.html` nav** — same pattern as Step 1.

- [ ] **Step 4: Update `shoproute/index.html` nav**

```html
    <a href="/#shipped">Work</a>
    <a href="/#tools">Tools</a>
    <a href="/#lab" aria-current="page">Lab</a>
```

- [ ] **Step 5: Update `lcars/index.html` nav** — same pattern as Step 4.

- [ ] **Step 6: Update `about/index.html` nav**

```html
    <a href="/#shipped">Work</a>
    <a href="/#tools">Tools</a>
    <a href="/#lab">Lab</a>
    <a href="/about/" aria-current="page">About</a>
```

- [ ] **Step 7: Check and update subpages**

```bash
grep -rn "nav class=\"nav\"" /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/integenius/ /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/kitchentime/ /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/solitairezero/
```

For each subpage that has a nav, add the Tools link in the same position.

- [ ] **Step 8: Verify Tools link is present in all files**

```bash
grep -rL "/#tools" /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/*.html /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/integenius/*.html /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/kitchentime/*.html /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/solitairezero/*.html /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/shoproute/*.html /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/lcars/*.html /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/chartula/*.html /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders/about/*.html 2>/dev/null
```

Expected: no output (all files contain `/#tools`). If any files are listed, add the nav link to them.

- [ ] **Step 9: Commit**

```bash
git -C /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders add integenius/index.html kitchentime/index.html solitairezero/index.html shoproute/index.html lcars/index.html about/index.html
git -C /Users/ryansaunders/Desktop/Dev/Websites/ryansaunders commit -m "feat(nav): add Tools link to all pages [RYAN]"
```

---

## Self-Review Checklist

- [x] CSS: `--accent-violet` tokens (dark + light) → Task 1, Steps 1–2
- [x] CSS: `.tool-tile` and `.tools` grid → Task 1, Step 5
- [x] CSS: `.module-strip` / `.module-card` → Task 1, Step 6
- [x] CSS: `.lab-tile.neon-arcade` → Task 1, Step 7
- [x] CSS: `.hero-image.nexus` gradient → Task 1, Step 4
- [x] CSS: `.meta .tools-count b` violet → Task 1, Step 3
- [x] CSS: `.title-block .meta-grid .running` → Task 1, Step 3
- [x] CSS: `.arcade-games` / `.arcade-game` → Task 4, Step 3
- [x] Homepage: Tools nav link → Task 2, Step 1
- [x] Homepage: hero counter chip → Task 2, Step 2
- [x] Homepage: Tools section HTML → Task 2, Step 3
- [x] Homepage: Lab renumbered to 03, Sonny removed, Neon Arcade added → Task 2, Step 4
- [x] Homepage: About → 04, Contact → 05 → Task 2, Steps 5–6
- [x] New page: `/nexus/` → Task 3
- [x] New page: `/neon-arcade/` → Task 4
- [x] Sonny rewrite → Task 5
- [x] Lab next-nav chain repaired → Task 6, Steps 1–4
- [x] Global nav update → Task 7
- [x] Screenshot placeholder comment in nexus/index.html → Task 3, Step 2 (note included)
