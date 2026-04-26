# RYAN-3: Add AI-relevant Skills Content

**Date:** 2026-04-26
**Status:** Design awaiting user review
**Ticket:** [RYAN-3](https://ryansaunders.atlassian.net/browse/RYAN-3), "Add AI-relevant skills content"
**Related spec:** [2026-04-20 portfolio redesign](2026-04-20-portfolio-redesign-design.md)

---

## 1. Goal

The portfolio site currently undersells the user's AI development workflow. The site mentions AI in passing on the About page, but does not communicate that the user runs a structured, multi-agent development pipeline with quality gates and a human review loop. RYAN-3 calls for surfacing that workflow in a way recruiters and engineering peers will recognize as systems-level thinking, not "I tried Copilot for a week."

The challenge: the original portfolio spec is explicit that the site has "no banners, no buzzwords, no 'AI-powered' labels," and that AI workflow content should appear "as a line about workflow, not a banner." This design resolves the tension by adding a small, on-brand prose section to the About page rather than a homepage feature, a new page, or a marketing block.

## 2. Scope

**In scope:**
- Edit `/about/index.html`:
  - Tighten the existing `02 How I work` paragraph.
  - Add a new `03 How AI fits in` section with three short paragraphs.

**Out of scope:**
- Homepage changes (hero copy, lede, Lab framing, etc.).
- New pages or routes (no `/workflow/`, no `/process/`).
- CSS changes. The existing `.prose` styles in `styles.css` already cover numbered headings and body paragraphs.
- Light/dark mode work.
- Updates to case-study pages, privacy pages, or Lab tiles.
- Adding a manual dark/light toggle, GitHub link in nav, or other items deferred from the original portfolio spec.

## 3. Voice and constraints

The new content lands somewhere between the site's existing voice (spare, plain, slightly dry) and the recruiter-facing language a hiring manager would search for. The hybrid:

- **Mostly the site's voice.** Short sentences. Concrete verbs. No marketing register. Reads like a person describing how they work, not a consultant pitching a methodology.
- **Two anchor terms allowed.** "Multi-agent workflow" appears once. "Human-in-the-loop" appears once. Each lands naturally inside a sentence, not as a header or a pill. These are the keywords a recruiter scanning for AI workflow experience will recognize.
- **Mid-level concreteness.** Name Claude (the substantive tool driving the workflow) and reference Jira and GitHub by category (board, pull requests). Do not enumerate the full ticket lifecycle, every quality gate, or specific Jira state names. The shape of the system should be clear; the wiring should not.
- **No em-dashes.** Use commas, periods, parentheses, or rephrasing. This is a standing user preference: em-dashes have become a strong "AI wrote this" signal and undermine the credibility of human-voice copy. Applies to all prose in the file, not just new prose. Existing copy in `02 How I work` will be tightened anyway, and the rewrite must not reintroduce em-dashes.

## 4. Page structure (after change)

The About page article body, in order:

1. `01 What I do`. Unchanged.
2. `02 How I work`. Tightened. One short paragraph on general working principles (user first, right tool, small units, iterate). The current paragraph's AI sentence is removed; that material moves to `03`.
3. `03 How AI fits in`. New. Three short paragraphs. See section 5 below.
4. Bottom CTA (`Get in touch` button). Unchanged.

The numbered-section rhythm (01 / 02 / 03) matches the homepage and case-study pages already established by the portfolio spec.

## 5. New `03 How AI fits in` section

Three paragraphs. Each one has a single job. Final words are the user's; drafts below are illustrative only and intended to show the shape and the kind of voice expected. The user will rewrite during implementation.

### Paragraph 1: what the setup is

**Job:** Frame the work as a system, not a tool. One or two sentences on how a piece of work moves from idea to PR. Mention Claude. Reference Jira (board / tickets) and GitHub (PRs) by category. Land "multi-agent workflow" once, naturally.

**Illustrative draft:**
> Most of my implementation work runs through a small multi-agent workflow. I write tickets on a Jira board, Claude agents pick them up, and the result comes back as a pull request on GitHub.

### Paragraph 2: what keeps it honest

**Job:** Describe the quality gates without enumerating them all. Make clear the reviewer role is the user's, by hand, on every PR. Land "human-in-the-loop" once, naturally.

**Illustrative draft:**
> Tests have to pass and formatting has to be clean before a pull request opens. After that, I review every change myself. The agents are fast, but the human-in-the-loop step is where the architecture stays coherent and the code stays mine.

### Paragraph 3: why it matters and how it scales

**Job:** One sentence on what the setup unlocks (rapid prototyping, safe refactors of older code). One sentence acknowledging the same shape works for a team, not just for solo work. This is the "don't sound like an elaborate personal sandbox" beat.

**Illustrative draft:**
> The point is leverage with judgment, not output for its own sake. The same shape (tickets, gates, review) is how a team can adopt AI in production without the output drifting, and it is how I keep my own projects moving without losing the parts I care about.

### Length and weight

Each paragraph is two to three sentences. Total visual weight on the page is similar to the existing `01 What I do` block. The page should still feel like a short personal page, not a process essay.

## 6. Tightened `02 How I work`

**Job:** Keep the general working principles the original portfolio spec called for ("small units, tested, iterated") but remove the AI sentence so `03` can carry that material without competition.

**Illustrative draft (final words are the user's):**
> The user comes first. I build small units, test what matters, iterate, and lean on the team. Right tool for the job, right amount of process for the project.

## 7. Acceptance criteria

- `/about/index.html` contains a new `03 How AI fits in` section with three paragraphs, sitting between the current `02 How I work` and the existing `Get in touch` button.
- The new section uses the existing `.prose` markup pattern (numbered `<h2>` with `.num` span, followed by `<p>` blocks). No CSS changes required.
- The current `02 How I work` paragraph has been tightened. The AI-related sentence in it has been removed.
- The phrase "multi-agent workflow" appears once on the page. The phrase "human-in-the-loop" appears once on the page.
- No em-dash characters appear anywhere in `/about/index.html` after the change.
- The page renders correctly in both dark mode and light mode in a current Safari and a current Chrome (visual QA only, no automated test required for this static page).

## 8. Implementation notes

- One file touched: `/about/index.html`.
- No JavaScript changes.
- No `styles.css` changes.
- No new assets.
- The Jira workflow rules in `CLAUDE.md` apply: feature branch `feature/RYAN-3-...`, single PR, summary comment on the ticket. The CLAUDE.md note about Swift-specific tooling (`make fmt`, Swift tests) does not apply to this static HTML site; no formatter or test suite to run.

## 9. Open questions

None blocking. Final wording for both the tightened `02` and the new `03` is the user's call during implementation; the spec defines the shape, not the prose.
