# Copy Checklist — Desk Accessory rebuild

Everything with blank/placeholder text, organized by page. `↻` marks copy that's **written once and reused** — do those first.

---

## 0 · Global — write once, reused everywhere (`↻`)

- [ ] **Footer tagline** — currently `built plain · loads fast`. `↻` all 18 pages.
- [ ] **Meta field labels** — `Status` / `Year` / `Platform` / `Next up`. `↻` all 10 project pages (labels only; values are per-project).
- [ ] **Contact form labels + button** — `Your email`, `Your message`, submit text (e.g. `Send`). `↻` all 3 contact pages **and** the homepage contact form (4 forms).
- [ ] **App Store button label** — e.g. `View on App Store`. `↻` every shipped project page + homepage.
- [ ] **Crumb labels** (by category) — `Back to Work` (shipped), `Back to Lab` (lab), `Back to Tools` (nexus/sonny), `Back to Home` (privacy/contact).
- [ ] **Case-study section-heading set** — the recurring headings, e.g. `What it is` / `Why it exists` / `What's next`. `↻` reused as the convention across all case studies (bodies differ).

---

## 1 · Homepage (`index.html`) — mostly drafted, finalize

- [ ] **Intro paragraph** — replace the `Placeholder intro; real copy comes later.` line.
- [ ] **Role line** — `iOS developer · Canada / remote` (confirm or reword).
- [ ] **Work rows ×3** — each: one-line blurb + status. `↻` the blurb + status feed each project's page (see §2).
- [ ] **Lab rows ×3** — same.
- Note: the old `Nexus` and `AI Stuff` homepage sections were dropped from this structure — re-add as rows if you want them back.

---

## 2 · Per-project spine — write once per project, reused (`↻`)

For each project, these atoms are shared between its **homepage row** and its **project page**:

- [ ] **One-liner** — `↻` homepage row blurb **=** project page title tag.
- [ ] **Status / Year / Platform / Next-up values** — `↻` homepage status **=** project meta grid.
- [ ] **Eyebrow** — `Project · Shipped`, `Lab · …`, or `Tools · …`.
- Prev/Next titles in the footer nav are just other project names — no new copy.

---

## 3 · Case-study bodies — unique per project

Each needs: eyebrow, tag, 4 meta values, the prose sections (heading is reusable, **body is unique**), plus the extras noted.

- [ ] **Integenius** — 4 sections, tech pills ×5, feature list (~7 bullets).
- [ ] **Kitchen Synk** — 3 sections, pills ×6, feature list (~8). *(before/after slider — no caption needed.)*
- [ ] **Solitaire Zero** — 4 sections, pills ×4, feature list (~7), **1 callout**.
- [ ] **Nexus** — 4 sections, pills ×8, feature list (~10), **module strip ×5** (name + blurb each), before/after slider.
- [ ] **Ed Assists** — 3 sections, pills ×6, **1 callout**.
- [ ] **Cardinal** — 3 sections, pills ×6, **1 callout**.
- [ ] **Neon Arcade** — 3 sections, pills ×6, **game cards ×2** (title + inspiration + blurb each).
- [ ] **Sonny** — 4 sections, pills ×6, feature list (~7).
- [ ] **LCARS** — 3 sections, pills ×5, **1 callout**.
- [ ] **ShopRoute** — 3 sections, pills ×6, **1 callout**.

---

## 4 · About page

- [ ] **Intro / bio** paragraph(s).
- [ ] **"What I do"** section body.
- [ ] **"How AI fits in"** section body.
- [ ] **Flow diagram ×4 nodes** — each a tool name + role label.

---

## 5 · Privacy — write ONE, reuse across 3 (`↻`)

Write a single policy; only the **app name** changes across integenius / kitchensynk / solitairezero.

- [ ] **Last-updated date** — `↻` all 3.
- [ ] **Policy body** (sections): Interpretation and Definitions (+ ~7 defined terms), Data We Do Not Collect (+ ~4 items), Data Stored on Your Device, Third Parties, Deleting Your Data, Children's Privacy, Security, Links to Other Websites, Changes to this Privacy Policy, Contact. `↻` all 3 (swap app name).

---

## 6 · Contact — write ONE, reuse across 3 (`↻`)

- [ ] **Intro line** — e.g. `Have a question or feedback about <App>? …`. `↻` all 3 (swap app name). *(Field labels + button are in §0.)*

---

### Suggested order (maximize reuse)
1. §0 globals → 2. §5 privacy template → §6 contact template → 3. §2 per-project spine (also fills homepage rows) → 4. §3 case-study bodies → 5. §1 homepage intro → §4 about.
