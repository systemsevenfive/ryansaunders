# Text-Minimal Rework Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Remove AI-style prose from the homepage and About page, replace the About teaser with a compact AI workflow callout linking to `/about/`, and add a flow diagram to the About page.

**Architecture:** Pure HTML/CSS changes across three files. No JS required. CSS additions only — no existing classes touched. The `.callout` class already exists in styles.css (pink left-border quote style); the new AI callout uses `.ai-callout` to avoid conflict.

**Tech Stack:** HTML5, CSS custom properties (all existing design tokens in `:root`). No build step — changes are live on save.

---

### Task 1: Add CSS variables and new styles to styles.css

**Files:**
- Modify: `styles.css` — add `--accent-violet-border` to both `:root` blocks, add AI callout + flow diagram classes at end of file

- [ ] **Step 1: Add `--accent-violet-border` to dark-mode `:root`**

In `styles.css`, inside the first `:root { }` block (around line 20, after `--accent-violet-soft`), add:

```css
  --accent-violet-border: rgba(167, 139, 250, 0.25);
```

- [ ] **Step 2: Add `--accent-violet-border` to light-mode `:root`**

In the `@media (prefers-color-scheme: light)` `:root` block (around line 55, after `--accent-violet-soft`), add:

```css
    --accent-violet-border: rgba(124, 58, 237, 0.25);
```

- [ ] **Step 3: Append AI callout and flow diagram styles to end of file**

After the final closing brace of `.submit-button:hover { transform: translateY(-1px); }`, append:

```css

/* ========== AI workflow callout (homepage) ========== */

.ai-callout {
  border: 1px solid var(--accent-violet-border);
  border-radius: var(--r-lg);
  padding: 22px 24px 20px;
  background: var(--accent-violet-soft);
  position: relative;
  overflow: hidden;
}
.ai-callout::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0; height: 2px;
  background: linear-gradient(90deg, var(--accent-violet), transparent);
  opacity: 0.7;
}
.mini-flow {
  display: flex;
  align-items: flex-start;
  flex-wrap: wrap;
  row-gap: 8px;
  margin-bottom: 16px;
}
.mini-node {
  display: flex;
  flex-direction: column;
  gap: 3px;
}
.mini-node .tool-name {
  font-size: 12px;
  font-weight: 600;
  color: var(--text);
}
.mini-node .tool-role {
  font-family: var(--font-mono);
  font-size: 9px;
  color: var(--text-faint);
  letter-spacing: 0.06em;
}
.mini-node.placeholder-node { opacity: 0.4; }
.mini-arrow {
  color: var(--accent-violet);
  font-size: 12px;
  padding: 0 12px;
  flex-shrink: 0;
  opacity: 0.5;
  margin-top: 2px;
}
.mini-gate-badge {
  display: inline-block;
  font-family: var(--font-mono);
  font-size: 8px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--accent-cyan);
  border: 1px solid rgba(0, 200, 255, 0.3);
  background: rgba(0, 200, 255, 0.08);
  border-radius: var(--r-sm);
  padding: 1px 5px;
}
.callout-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-top: 14px;
  border-top: 1px solid rgba(167, 139, 250, 0.15);
}
.callout-footer p {
  font-size: 13px;
  color: var(--text-muted);
}
.callout-footer a {
  font-family: var(--font-mono);
  font-size: 11px;
  letter-spacing: 0.04em;
  color: var(--accent-violet);
  text-decoration: none;
  flex-shrink: 0;
}

/* ========== AI workflow flow diagram (about page) ========== */

.flow-wrap {
  display: flex;
  align-items: stretch;
  margin-top: 20px;
  overflow-x: auto;
}
.flow-node {
  flex: 1;
  min-width: 120px;
  border: 1px solid var(--rule-strong);
  padding: 18px 14px 16px;
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.025), rgba(255, 255, 255, 0));
  display: flex;
  flex-direction: column;
  position: relative;
}
.flow-node:first-child { border-radius: var(--r-md) 0 0 var(--r-md); }
.flow-node:last-child  { border-radius: 0 var(--r-md) var(--r-md) 0; }
.flow-node + .flow-node { border-left: none; }
.flow-node:not(:last-child)::after {
  content: '';
  position: absolute;
  right: -9px; top: 50%; transform: translateY(-50%);
  width: 0; height: 0;
  border-top: 8px solid transparent;
  border-bottom: 8px solid transparent;
  border-left: 9px solid var(--rule-strong);
  z-index: 2;
}
.flow-node:not(:last-child)::before {
  content: '';
  position: absolute;
  right: -7px; top: 50%; transform: translateY(-50%);
  width: 0; height: 0;
  border-top: 6px solid transparent;
  border-bottom: 6px solid transparent;
  border-left: 7px solid var(--bg);
  z-index: 3;
}
.flow-node.gate {
  background: rgba(0, 200, 255, 0.04);
  border-color: rgba(0, 200, 255, 0.2);
}
.flow-node.gate::after { border-left-color: rgba(0, 200, 255, 0.2); }
.flow-node.placeholder-node {
  border-style: dashed;
  border-color: rgba(255, 255, 255, 0.1);
  background: transparent;
  opacity: 0.45;
}
.flow-node.placeholder-node::after { border-left-color: rgba(255, 255, 255, 0.1); }
.flow-eyebrow {
  font-family: var(--font-mono);
  font-size: 8px;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--accent-violet);
  margin-bottom: 8px;
}
.flow-node.gate .flow-eyebrow { color: var(--accent-cyan); }
.flow-node.placeholder-node .flow-eyebrow { color: var(--text-faint); }
.flow-node h3 {
  font-size: 13px;
  font-weight: 700;
  letter-spacing: -0.01em;
  margin-bottom: 5px;
}
.flow-node .flow-role {
  font-size: 11px;
  color: var(--text-muted);
  line-height: 1.5;
  flex: 1;
}
.flow-node.placeholder-node .flow-role {
  color: var(--text-faint);
  font-style: italic;
}
.gate-tag {
  display: inline-block;
  margin-top: 8px;
  font-family: var(--font-mono);
  font-size: 8px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--accent-cyan);
  border: 1px solid rgba(0, 200, 255, 0.3);
  background: rgba(0, 200, 255, 0.08);
  border-radius: var(--r-sm);
  padding: 2px 6px;
}

@media (prefers-color-scheme: light) {
  .flow-node {
    background: linear-gradient(180deg, rgba(0, 0, 0, 0.015), rgba(0, 0, 0, 0));
  }
  .flow-node.placeholder-node {
    border-color: rgba(0, 0, 0, 0.1);
  }
}
```

- [ ] **Step 4: Open `index.html` in a browser and confirm the page looks unchanged** (no new elements exist yet, just verifying the CSS additions didn't break anything)

- [ ] **Step 5: Commit**

```
git add styles.css
git commit -m "feat(styles): add ai-callout and flow-node classes"
```

---

### Task 2: Replace About teaser on homepage with AI callout

**Files:**
- Modify: `index.html` — replace contents of `.about-teaser` section

- [ ] **Step 1: Replace the `.about-teaser` section content**

In `index.html`, find and replace the entire `.about-teaser` section (lines 136–139):

**Remove:**
```html
  <section class="about-teaser">
    <p>As a lifelong tecchie, hacker, tinkerer, and general enthusiast for all things computer-related, I've never been more excited about the possibilities.</p>
    <a class="read-more" href="/about/">Read more →</a>
  </section>
```

**Replace with:**
```html
  <section class="about-teaser">
    <div class="ai-callout">
      <div class="mini-flow">
        <div class="mini-node">
          <span class="tool-name">Nexus</span>
          <span class="tool-role">Tickets</span>
        </div>
        <span class="mini-arrow" aria-hidden="true">→</span>
        <div class="mini-node">
          <span class="tool-name">Claude</span>
          <span class="tool-role">Agents</span>
        </div>
        <span class="mini-arrow" aria-hidden="true">→</span>
        <div class="mini-node">
          <span class="tool-name">Xcode · Git</span>
          <span class="tool-role"><span class="mini-gate-badge">Gate</span></span>
        </div>
        <span class="mini-arrow" aria-hidden="true">→</span>
        <div class="mini-node">
          <span class="tool-name">Review</span>
          <span class="tool-role">Every diff</span>
        </div>
        <span class="mini-arrow" aria-hidden="true">→</span>
        <div class="mini-node placeholder-node">
          <span class="tool-name">Local LLMs</span>
          <span class="tool-role">In progress</span>
        </div>
      </div>
      <div class="callout-footer">
        <p>iOS developer. Multi-agent workflow, human-reviewed.</p>
        <a href="/about/">About me →</a>
      </div>
    </div>
  </section>
```

- [ ] **Step 2: Open `index.html` in a browser and verify**

Check:
- The violet-accented callout card appears in the About section
- Mini pipeline reads: Nexus → Claude → Xcode · Git (with cyan Gate badge) → Review → Local LLMs (faded)
- "About me →" link is right-aligned in violet monospace
- Footer text "iOS developer. Multi-agent workflow, human-reviewed." appears left-aligned in muted tone
- No old teaser paragraph remains

- [ ] **Step 3: Check light mode**

In Safari: Develop > Enter Responsive Design Mode, or toggle System Preferences to light mode. Verify the callout is readable — violet tint on light background should still be distinguishable.

- [ ] **Step 4: Commit**

```
git add index.html
git commit -m "feat(home): replace about teaser with ai workflow callout"
```

---

### Task 3: Trim About page and add flow diagram

**Files:**
- Modify: `about/index.html` — remove "How I work", trim "What I do", renumber + trim "How AI fits in", add flow diagram

- [ ] **Step 1: Replace the entire `.prose` section**

In `about/index.html`, find and replace the entire `<section class="prose">` block:

**Remove:**
```html
    <section class="prose">
      <h2><span class="num">01</span>What I do</h2>
      <p><em>iOS Developer:</em> It's my day job and my day-off job. Most of my life has been spent in the Apple ecosystem, as a student, a creative, and a technical professional. I like building good software that follows good practices, and I like building rapid prototypes to push the limits. </p>

      <h2><span class="num">02</span>How I work</h2>
      <p><em>The user comes first.</em> I build small, I test what matters, I iterate, and I lean on the team. Right tool for the job, right amount of process for the project.</p>

      <h2><span class="num">03</span>How AI fits in</h2>
      <p><em>Multi-agent workflow.</em> My implementation work moves through a small multi-agent workflow. Tickets live on a Jira board, Claude agents pick them up, and the result comes back as a pull request on GitHub.</p>
      <p><em>Quality gates.</em> Tests have to pass and formatting has to be clean before a pull request can open. I review every change myself. The agents are fast, but the human-in-the-loop step is where the architecture stays coherent and the code stays mine.</p>
      <p><em>Leverage with judgment.</em> The same shape (tickets, gates, review) is how a team can adopt AI in production without the output drifting, and how I keep my own projects moving without losing the parts I care about.</p>

    </section>
```

**Replace with:**
```html
    <section class="prose">
      <h2><span class="num">01</span>What I do</h2>
      <p>iOS developer — day job and day-off job. Most of my life spent in the Apple ecosystem, as a student, a creative, and a technical professional.</p>

      <h2><span class="num">02</span>How AI fits in</h2>
      <p>Multi-agent workflow. Tickets live in Nexus. Claude agents pick them up and return pull requests. Build and tests must pass before a PR opens — no exceptions. I review every diff. The architecture decisions stay mine.</p>

      <div class="flow-wrap">
        <div class="flow-node">
          <div class="flow-eyebrow">Nexus</div>
          <h3>Ticket</h3>
          <p class="flow-role">Scope and acceptance criteria defined before any code runs.</p>
        </div>
        <div class="flow-node">
          <div class="flow-eyebrow">Claude</div>
          <h3>Agent</h3>
          <p class="flow-role">Picks up the ticket, writes the implementation, opens a draft PR.</p>
        </div>
        <div class="flow-node gate">
          <div class="flow-eyebrow">Xcode · Git</div>
          <h3>Gate</h3>
          <p class="flow-role">Build passes. Tests pass. Formatting is clean.</p>
          <span class="gate-tag">Required</span>
        </div>
        <div class="flow-node">
          <div class="flow-eyebrow">GitHub</div>
          <h3>Review</h3>
          <p class="flow-role">I read every diff. Architecture decisions stay mine.</p>
        </div>
        <div class="flow-node placeholder-node">
          <div class="flow-eyebrow">Local LLMs</div>
          <h3>On-device</h3>
          <p class="flow-role">Private or latency-sensitive inference. In progress.</p>
        </div>
      </div>
    </section>
```

- [ ] **Step 2: Open `about/index.html` in a browser and verify**

Check:
- Only two numbered sections remain: 01 What I do, 02 How AI fits in
- "How I work" is gone
- "What I do" is two sentences, no `<em>` callout styling
- Flow diagram appears below the AI paragraph: five connected nodes with arrow chevrons between them
- Gate node (Xcode · Git) has cyan tint and a `REQUIRED` badge
- Local LLMs node is faded with dashed border
- No old three-paragraph AI section text remains

- [ ] **Step 3: Check light mode**

Toggle system to light mode. Verify:
- Flow nodes are readable (gradient background adapts via the light-mode media query added in Task 1)
- Arrow chevrons are visible (they use `var(--bg)` fill which adapts to `#faf9f6` in light mode)
- Gate node is still legible

- [ ] **Step 4: Commit**

```
git add about/index.html
git commit -m "feat(about): remove how-i-work, trim what-i-do, add flow diagram"
```
