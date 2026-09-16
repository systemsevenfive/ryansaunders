# Desk Accessory Homepage Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Restyle the blank-slate copy's homepage to the approved Desk Accessory design — a fast, responsive, system-themed page that folds two System 7.5 elements (pinstripe masthead, Finder list rows) into a real website.

**Architecture:** Two files change in `ryansaunders-copy/`: a full rewrite of the shared `styles.css` (token system + shared components) and a rebuild of `index.html` to the reference markup. No framework, no build step, no web fonts, no JavaScript. Theme is system-driven via `prefers-color-scheme`.

**Tech Stack:** Plain HTML5 + one CSS file. Verification via `python3 -m http.server` and the Browser pane (responsive presets + `colorScheme` emulation + network panel).

**Medium adaptations (read first):**
- No unit-test runner exists for a static site. "Verify" steps are (a) `grep` structural assertions and (b) browser render checks with expected observations — these are the tests.
- The copy has **no `.git`** (deliberate). "Checkpoint" steps replace commits; there is nothing to commit.
- Reference implementation is the approved mockup artifact "Desk Accessory". The code below is final — port it verbatim.

**Out of scope / deferred (surface, don't silently drop):**
- The old homepage's `NEXUS` and `AI STUFF` sections are **not** in the approved structure (Work / Lab / Contact only). Revisit in the second pass or fold Nexus in as a row then.
- The other 17 pages (about, 10 project pages, privacy/contact boilerplate) are **not** restyled here. After `styles.css` is replaced they render on base typography only (legible, plain) until the second pass. This is expected.

---

## File structure

- `ryansaunders-copy/styles.css` — full rewrite. Owns: color/type tokens (light + dark), global base, and every shared component (top bar, hero, section label, list rows, button, footer). Consumed by all pages.
- `ryansaunders-copy/index.html` — rebuild. Owns: the homepage markup consuming `styles.css`.

No other files are created or modified in this pass.

---

### Task 1: Rewrite the shared stylesheet

**Files:**
- Modify (full replace): `ryansaunders-copy/styles.css`

- [ ] **Step 1: Overwrite `styles.css` with the Desk Accessory system**

Replace the entire file with exactly this:

```css
/* ==========================================================================
   Desk Accessory — ryansaunders.com
   Polished low-tech: two System 7.5 elements (pinstripe masthead, Finder
   list rows) on a fast, responsive, system-themed page. No web fonts, no JS.
   Spec: docs/superpowers/specs/2026-09-12-desk-accessory-redesign-design.md
   ========================================================================== */

:root{
  color-scheme: light dark;
  --paper:#F4F2EA; --ink:#1B1A17; --dim:#6C6959; --line:#CBC6B8; --line-2:#B7B2A3;
  --accent:#3F5E86; --btn-hi:#FFFFFF; --btn-sh:#CFCABA; --sel:#DDE6F0;
  --mono:ui-monospace,"SF Mono",Menlo,Monaco,"Cascadia Mono",monospace;
  --sans:system-ui,-apple-system,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
}
@media (prefers-color-scheme:dark){
  :root:not([data-theme="light"]){
    --paper:#15151A; --ink:#E7E4DB; --dim:#938F80; --line:#33333A; --line-2:#43434C;
    --accent:#8FB0DC; --btn-hi:#33333A; --btn-sh:#0D0D10; --sel:#25303F;
  }
}
:root[data-theme="dark"]{
  --paper:#15151A; --ink:#E7E4DB; --dim:#938F80; --line:#33333A; --line-2:#43434C;
  --accent:#8FB0DC; --btn-hi:#33333A; --btn-sh:#0D0D10; --sel:#25303F;
}

*{box-sizing:border-box;}
html{background:var(--paper);}
body{margin:0; background:var(--paper); color:var(--ink); font-family:var(--sans);
  font-size:16px; line-height:1.65; -webkit-font-smoothing:antialiased;}
::selection{background:var(--sel);}
a{color:var(--accent); text-underline-offset:3px; text-decoration-thickness:1px;}
a:focus-visible,.btn:focus-visible{outline:2px solid var(--accent); outline-offset:2px;}

.wrap{max-width:720px; margin:0 auto; padding:0 clamp(18px,5vw,28px);}

/* masthead — the one title-bar nod */
.top{position:sticky; top:0; z-index:5; background:var(--paper); border-bottom:1px solid var(--line);}
.top .wrap{display:flex; align-items:center; gap:14px; flex-wrap:wrap; padding-top:11px; padding-bottom:11px;}
.brand{display:flex; align-items:center; gap:9px; font-weight:600; letter-spacing:.01em; color:var(--ink); text-decoration:none;}
.cbox{width:10px; height:10px; border:1.5px solid var(--ink); border-radius:2px; flex:none;}
.pin{width:22px; height:15px; flex:none; border:1px solid var(--line-2); border-radius:2px;
  background:repeating-linear-gradient(var(--ink) 0 1px, transparent 1px 3px);}
nav.links{margin-left:auto; display:flex; gap:16px; font-family:var(--mono); font-size:12.5px; letter-spacing:.04em; text-transform:lowercase;}
nav.links a{color:var(--dim); text-decoration:none;}
nav.links a:hover{color:var(--ink);}

/* header */
.hero{padding:38px 0 8px;}
.hero h1{margin:0; font-size:clamp(26px,6vw,34px); line-height:1.1; letter-spacing:-.015em; text-wrap:balance;}
.role{margin:8px 0 0; font-family:var(--mono); font-size:12.5px; letter-spacing:.06em; text-transform:uppercase; color:var(--dim);}
.intro{margin:16px 0 0; max-width:60ch; color:var(--ink);}
.intro .muted{color:var(--dim);}

/* section label + list */
section{padding:30px 0 4px;}
.lbl{display:flex; align-items:center; gap:10px; font-family:var(--mono); font-size:11.5px; letter-spacing:.16em; text-transform:uppercase; color:var(--dim); margin:0 0 4px;}
.lbl::after{content:""; flex:1; height:1px; background:var(--line);}

.list{border-top:1px solid var(--line);}
.row{display:grid; grid-template-columns:minmax(0,1fr) minmax(0,1.5fr) auto; gap:6px 18px; align-items:baseline; padding:13px 0; border-bottom:1px solid var(--line);}
.row a.nm{font-weight:600; font-size:17px; color:var(--ink); text-decoration:none;}
.row a.nm:hover{color:var(--accent); text-decoration:underline;}
.row .desc{color:var(--dim); font-size:14.5px;}
.row .status{font-family:var(--mono); font-size:12px; color:var(--dim); white-space:nowrap; justify-self:end;}
.row .status .live{color:var(--accent);}
@media (max-width:560px){
  .row{grid-template-columns:1fr auto; row-gap:3px;}
  .row .desc{grid-column:1 / -1; order:3;}
  .row .status{order:2;}
}

/* contact + button */
.contact{padding:30px 0 6px;}
.btn{display:inline-block; margin-top:14px; font-family:var(--mono); font-size:12.5px; letter-spacing:.04em; color:var(--ink); text-decoration:none;
  background:var(--paper); border:1px solid var(--line-2); border-radius:7px; padding:8px 16px; box-shadow:inset 1px 1px 0 var(--btn-hi), inset -1px -1px 0 var(--btn-sh);}
.btn:hover{border-color:var(--dim);}

footer{margin-top:44px; border-top:1px solid var(--line);}
footer .wrap{display:flex; justify-content:space-between; gap:12px; flex-wrap:wrap; padding-top:16px; padding-bottom:40px; font-family:var(--mono); font-size:11.5px; color:var(--dim);}
```

- [ ] **Step 2: Assert no external resources and correct tokens**

Run: `cd ~/Desktop/Dev/Websites/ryansaunders-copy && grep -nE "@import|fonts\.googleapis|http|url\(" styles.css`
Expected: **no output** (exit 1). The stylesheet loads nothing external.

Run: `grep -c -- "--paper" styles.css`
Expected: `3` (defined in `:root`, dark media block, and `[data-theme="dark"]`).

- [ ] **Step 3: Checkpoint**

No git in this copy. Proceed to Task 2 — visual verification happens in Task 3 once the markup exists.

---

### Task 2: Rebuild the homepage markup

**Files:**
- Modify (full replace): `ryansaunders-copy/index.html`

- [ ] **Step 1: Overwrite `index.html` with the reference markup**

Replace the entire file with exactly this:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ryan Saunders — iOS Developer</title>
  <meta name="description" content="">
  <link rel="stylesheet" href="/styles.css">
</head>
<body>

<div class="top">
  <div class="wrap">
    <a class="brand" href="#top">
      <span class="cbox" aria-hidden="true"></span>
      <span class="pin" aria-hidden="true"></span>
      Ryan Saunders
    </a>
    <nav class="links" aria-label="Primary">
      <a href="#work">work</a>
      <a href="#lab">lab</a>
      <a href="/about/">about</a>
      <a href="#contact">contact</a>
    </nav>
  </div>
</div>

<a id="top"></a>
<header class="hero">
  <div class="wrap">
    <h1>Ryan Saunders</h1>
    <p class="role">iOS developer · Canada / remote</p>
    <p class="intro">I build small, careful apps for iPhone, iPad, and Mac — and a lot of tools for myself along the way. <span class="muted">Placeholder intro; real copy comes later.</span></p>
  </div>
</header>

<main>
  <section id="work">
    <div class="wrap">
      <p class="lbl">Work</p>
      <div class="list">
        <div class="row"><a class="nm" href="/integenius/">Integenius</a><span class="desc">Arithmetic flash cards for the iPad generation.</span><span class="status"><span class="live">●</span> live</span></div>
        <div class="row"><a class="nm" href="/kitchensynk/">Kitchen Synk</a><span class="desc">Parallel cooking timers for the four-burner dinner.</span><span class="status"><span class="live">●</span> live</span></div>
        <div class="row"><a class="nm" href="/solitairezero/">Solitaire Zero</a><span class="desc">Klondike solitaire, no fuss, for free.</span><span class="status"><span class="live">●</span> live</span></div>
      </div>
    </div>
  </section>

  <section id="lab">
    <div class="wrap">
      <p class="lbl">In the lab</p>
      <div class="list">
        <div class="row"><a class="nm" href="/edassists/">Ed Assists</a><span class="desc">A hand for the video editors of the world.</span><span class="status">○ prototype</span></div>
        <div class="row"><a class="nm" href="/cardinal/">Cardinal</a><span class="desc">The missing link between Contacts and Calendars.</span><span class="status">○ tinkering</span></div>
        <div class="row"><a class="nm" href="/neon-arcade/">Neon Arcade</a><span class="desc">Homage to the classics, written in Swift.</span><span class="status">○ in dev</span></div>
      </div>
    </div>
  </section>

  <section id="contact" class="contact">
    <div class="wrap">
      <p class="lbl">Contact</p>
      <p class="intro"><a href="https://www.linkedin.com/in/systemseven/" target="_blank" rel="noopener noreferrer">linkedin</a> · <a href="#contact">email</a></p>
      <a class="btn" href="#">View on App Store</a>
    </div>
  </section>
</main>

<footer>
  <div class="wrap">
    <span>© 2026 · Ryan Saunders</span>
    <span>built plain · loads fast</span>
  </div>
</footer>

</body>
</html>
```

- [ ] **Step 2: Assert no shell assets and correct wiring**

Run: `cd ~/Desktop/Dev/Websites/ryansaunders-copy && grep -nE "<script|<img" index.html`
Expected: **no output** (exit 1). No JavaScript, no images in the shell.

Run: `grep -n "/styles.css" index.html`
Expected: one line — the `<link rel="stylesheet" href="/styles.css">`.

Run: `grep -n 'aria-label="Primary"' index.html`
Expected: one line — the nav landmark is labeled.

- [ ] **Step 3: Checkpoint**

Files are in place. Proceed to render verification.

---

### Task 3: Verify rendering — themes and breakpoints

**Files:** none (verification only).

- [ ] **Step 1: Serve the copy locally**

Run (background): `cd ~/Desktop/Dev/Websites/ryansaunders-copy && python3 -m http.server 8091`
Then open the Browser pane to `http://localhost:8091/` (via `preview_start` url or `navigate`).
Expected: the homepage renders — sticky bar with close-box + pinstripe + "Ryan Saunders", Work and Lab list rows, Contact, footer.

- [ ] **Step 2: Desktop, light**

Resize the pane to desktop (or width 1200) with `colorScheme: light`. Screenshot.
Expected: warm off-white ground, dark ink, single blue accent on the `●` live dots and links. Reading column capped (does not stretch full width). Section labels are mono uppercase with a hairline rule filling the row.

- [ ] **Step 3: Desktop, dark**

Set `colorScheme: dark`, same width. Screenshot.
Expected: warm near-black ground `#15151A`, light ink, lifted blue accent. Every text element legible; hairlines visible but quiet. No element keeps a light-theme color.

- [ ] **Step 4: Tablet (768) and large monitor (1440)**

Resize to the tablet preset, then width 1440. Screenshot each.
Expected: layout unchanged in structure; padding grows via `clamp`; the 720px column stays centered with generous margins on 1440 (no sprawl).

- [ ] **Step 5: Phone (375) — row collapse**

Resize to the mobile preset (375). Screenshot.
Expected: each list row shows **name + status on the first line** and the **blurb full-width beneath** it. Nav wraps below the wordmark if needed. No horizontal page scroll.

- [ ] **Step 6: Confirm zero external requests**

Read the Browser pane network requests for the page load.
Expected: only `localhost:8091` requests (the document and `/styles.css`). No fonts.googleapis, no image hosts, no third-party.

- [ ] **Step 7: Checkpoint**

If any check fails, fix the offending rule in `styles.css` (or markup in `index.html`) and re-run the relevant step. Do not proceed until Steps 2–6 all pass.

---

### Task 4: Deferred-pages sanity

**Files:** none (verification only).

- [ ] **Step 1: Confirm other pages stay legible on base styles**

With the server still running, open `http://localhost:8091/about/` and `http://localhost:8091/integenius/`.
Expected: plain but readable — system font, ink on paper, no broken layout or invisible (light-on-light / dark-on-dark) text. They are intentionally unstyled beyond the global base until the second pass; confirm nothing is unreadable.

- [ ] **Step 2: Confirm no console errors**

Read the Browser pane console for the homepage.
Expected: no errors (there is no JS; a clean console).

- [ ] **Step 3: Stop the server**

Stop the background `http.server` (or the `preview_start` server).

---

### Task 5: Handoff to Ryan

**Files:** none.

- [ ] **Step 1: Put the built page in front of Ryan for review**

Publish the built homepage as an artifact **or** point Ryan at the local server, and summarize (point form): what changed, breakpoints verified, light/dark verified, deferred-pages note. Ryan reviews the real built page before the second pass (the 17 remaining pages) is planned.

---

## Notes for the implementer

- Class names are shared between `styles.css` and `index.html`: `.top .wrap`, `.brand`, `.cbox`, `.pin`, `nav.links`, `.hero`, `.role`, `.intro`, `.lbl`, `.list`, `.row`, `a.nm`, `.desc`, `.status`, `.live`, `.contact`, `.btn`, `footer .wrap`. Keep them in sync.
- `--accent` is the only non-neutral color. Do not introduce a second accent.
- Do not add web fonts, icons, images, or JavaScript to satisfy any step — the constraints are the point.
