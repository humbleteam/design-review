<div align="center">

<h1>Design review</h1>

**Turn a screenshot, URL, or HTML snippet into a scored UX critique with a citation behind every claim - a Claude Code skill built for design teams and the engineers who ship their work.**

[![CI](https://img.shields.io/github/actions/workflow/status/humbleteam/design-review/validate.yml?branch=main&style=for-the-badge&logo=github&label=CI)](https://github.com/humbleteam/design-review/actions/workflows/validate.yml)
[![GitHub stars](https://img.shields.io/github/stars/humbleteam/design-review?style=for-the-badge&logo=github&color=181717)](https://github.com/humbleteam/design-review/stargazers)
[![Last commit](https://img.shields.io/github/last-commit/humbleteam/design-review?style=for-the-badge&color=339933)](https://github.com/humbleteam/design-review/commits/main)
[![License](https://img.shields.io/badge/license-MIT-blue?style=for-the-badge)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude_Code-skill-D97757?style=for-the-badge&logo=anthropic&logoColor=white)](https://github.com/humbleteam/design-review/blob/main/SKILL.md)

</div>

design-review takes a UI screenshot, a live URL, or an HTML snippet and returns a structured critique: a 0-4 ship-readiness score, 3 to 6 prioritized issues, and a Before/After/Why line for each one. The rule that keeps it useful is a citation floor - every fix names a Nielsen heuristic, a WCAG 2.2 success criterion, or a named platform guideline, never a bare opinion. Hand it two or more variants and it switches to comparison mode: a score each, a shared-dimension table, and a named winner. With no artifact at all, it switches to advisory mode: one direct recommendation, cited reasoning.

## Table of contents

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Usage](#usage)
- [Example output](#example-output)
- [How it works](#how-it-works)
- [How is this different from just asking the model?](#how-is-this-different-from-just-asking-the-model)
- [FAQ](#faq)
- [Related skills](#related-skills)
- [Who maintains this](#who-maintains-this)

## What it does

- Reviews a screenshot, a live URL, or an HTML snippet and returns a 0-4 ship-readiness score.
- Picks the 3-6 most impactful issues instead of listing every flaw, so the critique stays actionable.
- Writes each issue as Before / After / Why - an observable fact, a fix doable in under an hour, one citation.
- Compares two or more variants on shared dimensions and names a winner, instead of returning two separate critiques and no answer.
- Switches to advisory mode for open decision questions ("modal or a full page for account deletion?") with no artifact attached.
- Answers a decision question that arrives with a screen attached, using the screen as evidence, instead of handing back a critique that never reaches the question.
- Asks for a better screenshot or a working URL when the input is unreadable.
- Still surfaces 2-3 polish items on a design that would score 4/4 - never an empty pass.

## Quick start

**Personal - available in every project:**

```bash
git clone https://github.com/humbleteam/design-review ~/.claude/skills/design-review
```

**Project-scoped - checked into one repo:**

```bash
git clone https://github.com/humbleteam/design-review .claude/skills/design-review
```

**Any other agent:** the skill is plain markdown in the Agent Skills format. Paste `SKILL.md` into the system prompt of Cursor, Codex, or any other LLM agent.

To verify: restart Claude Code and ask it to list its skills. Skills load from `~/.claude/skills/` (personal) and `.claude/skills/` (project) - `design-review` should appear.

## Usage

- **"Review this screenshot of our onboarding flow."** (image attached) - review mode: scores it 0-4, returns 3-6 cited issues.
- **"What's wrong with https://example.com/pricing?"** - review mode against a live URL instead of a screenshot.
- **"Which of these two checkout variants is better?"** (two images attached) - comparison mode: a score each, a dimension table, and a named winner.
- **"Should I use a modal or a full-page flow for account deletion?"** - no artifact, so advisory mode: one recommendation, up to 5 cited bullets.
- **"Here's our checkout - should the address form be a modal or a drawer?"** (one image attached) - grounded advisory mode: the question gets answered first, with facts read off the screen as the evidence.

## Example output

Example review of a fictional signup screen - product name and screen are invented for illustration:

```
## Signup screen - score 2/4

### 1. Primary action competes with a secondary link
- Before: "Create account" and "Log in" share the same font weight.
- After: make "Create account" a filled button; drop "Log in" below it as
  plain text, no border.
- Why: Nielsen heuristic #8 - aesthetic and minimalist design - two actions
  compete when only one is primary.

### 2. Password field gives no format guidance until after submit
- Before: requirements (8+ characters, one number) only show as an error
  after a failed submit.
- After: show them as static helper text under the field before typing.
- Why: WCAG 2.2 SC 3.3.5 (help) - requirements should be visible before an
  error occurs, not only after.

### 3. Form fields are unlabeled placeholder text
- Before: "Email" and "Password" exist only as placeholder text and vanish
  once the user starts typing.
- After: add persistent labels above each field; keep placeholder as an
  example value only.
- Why: Nielsen heuristic #6 - recognition rather than recall - a vanished
  placeholder forces recall instead of recognition.

Fix this first: the missing password guidance is the highest-cost fix - most
likely to cause a failed submit, and a one-line copy change.
```

A decision question with a screen attached returns the other shape - the answer first, the
critique only if it bears on the decision. Prompt: "Here's our checkout. Should the address
form be a modal or a drawer?" (screenshot attached):

```
Recommendation: keep the address form inline on the checkout page; neither a
modal nor a drawer earns its cost here.

- The form is 9 fields deep and the page already carries a sticky order
  summary, so an overlay would cover the one element people check while
  typing an address. Why: Nielsen heuristic #1 - visibility of system status
  - the total is the status people are tracking during checkout.
- A drawer at this height scrolls inside its own container on a 390px-wide
  screen, which puts two scroll regions on one screen. Why: Apple Human
  Interface Guidelines - sheets - a sheet that scrolls internally competes
  with the page behind it.
- General guidance, not visible in this screenshot: if most of your traffic
  is returning customers with a saved address, the question changes - the
  form becomes a confirm step, and a modal is defensible.

What in the artifact decided it: the sticky order summary sits in the region
a modal or drawer would cover.

Also worth fixing: the "Continue" button is below the fold on the attached
viewport, which hides the exit from the step being discussed.
```

## How it works

- **Mode detection first, in a fixed order.** Two or more artifacts plus a "which one wins" question triggers comparison mode; an artifact plus a decision question triggers grounded advisory mode; one artifact and no decision question triggers review mode; a decision question with no artifact triggers advisory mode.
- **The rubric loads before the critique.** `references/review-rubric.md` holds the exact 0-4 bands, the tie-break for a screen that fits two of them, and the citation table, so scoring stays consistent run to run.
- **Score before listing issues.** 0 is broken, 4 is ship-ready. Score generously when the design serves the stated project goals; harshly when it ignores them.
- **Cap the issue list at 6, ranked by impact** - listing every flaw is a failure mode, not thoroughness.
- **Before is a fact, never a feeling:** "12 elements inside a 320px card with no grouping," not "this feels cluttered."
- **After fits inside an hour** - concrete enough to hand to a developer with no follow-up question.
- **Why is exactly one citation:** a Nielsen heuristic, a WCAG 2.2 success criterion, or a named platform guideline - never an uncited opinion.
- **The close names the one fix that matters most**, not a generic summary.
- **Comparison mode judges variants against each other**, not one after the other: 3-5 shared dimensions, one row per dimension, an edge called on each, and a named winner with the one fact that would flip it.
- **Advisory mode compresses the same discipline** into one recommendation plus up to 5 cited bullets, asking one clarifying question instead of guessing when a request is too open.
- **An attached screen raises the evidence bar, it does not change the question.** In grounded advisory mode every claim the screen can settle names an observable fact from it, the one fact that carried the call is stated outright, and anything the screen cannot settle is labeled as general guidance rather than dressed up as an observation.

## How is this different from just asking the model?

A bare "review this design" prompt returns a wall of adjectives - "the hierarchy feels off," "the spacing could be tighter" - with no way to tell which comment matters most or where it came from. This skill pins the output shape down: a score, a capped issue count, a Before/After/Why line per item, so every run is the same structure and easy to compare. It also pins the citation - a fix traces back to a numbered heuristic, not "best practice" in the abstract. It does not replace judgment about your users or a usability test.

## FAQ

**Can AI do a design review?**
Yes, with limits. This skill scores a screenshot, URL, or HTML snippet against Nielsen's 10 heuristics and WCAG 2.2. It reads pixels and markup, not user behavior, so it catches heuristic and accessibility gaps but cannot replace a usability test.

**How do I review a Figma design with Claude?**
Screenshot the frame and paste it into Claude Code, then ask for a review. A pasted screenshot is treated the same as a live URL: a 0-4 score plus 3-6 cited issues.

**What is a good design review checklist?**
At minimum, Nielsen's 10 heuristics: system status, real-world match, user control, consistency, error prevention, recognition over recall, flexibility, minimalist design, error recovery, help. `references/review-rubric.md` has the full table.

**How do I score design quality?**
This skill uses a 0-4 scale: 0 is broken (basic accessibility, hierarchy, or trust violations), 4 is ship-ready. Bands live in `references/review-rubric.md`, along with the rule for the common case where two bands fit the same screen: the cost of the fix decides - a rebuild, a restructure, a craft pass, one to three discrete fixes, or nothing material - and a count of violations is evidence rather than a threshold, so five small ones on a sound structure score better than a single one that blocks the task. Score generously when a design serves its stated goals, harshly when it ignores them.

**Can this review a live website instead of a screenshot?**
Yes, give it a URL. If the URL is unreachable - an auth wall or a 404 - it asks for a screenshot instead of guessing.

**How do I compare two design variants and pick a winner?**
Attach both and ask which one wins. That is comparison mode: each variant gets its own 0-4 score, then 3-5 shared dimensions are judged side by side in a table, and one variant is named the winner along with the fact that would change the call. It does not hand back two separate reviews, because two reviews still leave you to make the decision yourself. The same mode handles a before-and-after pair when you want to know whether a redesign actually improved anything.

**What if I don't have a design yet, just a decision to make?**
That's advisory mode: a direct question like "modal or full page for account deletion?" gets one recommendation plus up to 5 cited bullets.

**I have a screenshot and a decision to make - which mode is that?**
Grounded advisory. The question decides the mode and the screen becomes the evidence, so you get the answer first, with the observable facts it rests on, and at most three fixes that bear on that decision. It is deliberately not a review: a critique that never reaches the question you asked is a well-formed way of not answering. Ask for a full review separately and you get the score and the 3-6 issues as well. If the screen does not show the thing you asked about, it says so instead of inferring it from what is around it.

## Related skills

Part of a 10-skill open-source kit for design teams by Humbleteam.

- [ascii-wireframes](https://github.com/humbleteam/ascii-wireframes) - three distinct layout hypotheses as ASCII wireframes before any hi-fi work.
- [html-mockup](https://github.com/humbleteam/html-mockup) - census-first HTML mockups that match a reference screenshot: exact palette, item counts, component states.
- [extract-design-tokens](https://github.com/humbleteam/extract-design-tokens) - pull palette, type, spacing, radii, and shadows from a URL or screenshot into CSS variables and JSON.
- [audit-design-tokens](https://github.com/humbleteam/audit-design-tokens) - find token drift in a codebase: raw hex values, off-scale spacing, near-duplicate colors.
- [design-qa](https://github.com/humbleteam/design-qa) - a pre-ship design QA gate: states, contrast, touch targets, breakpoints, keyboard paths.
- [design-handoff](https://github.com/humbleteam/design-handoff) - turn a finished mockup into a dev-ready spec: tokens, states, accessibility annotations, open questions.
- [accessibility-audit](https://github.com/humbleteam/accessibility-audit) - WCAG 2.2-grounded accessibility review with success-criterion citations and severity levels.
- [ux-writing](https://github.com/humbleteam/ux-writing) - interface copy that reads human: plain-verb microcopy rules and an AI-tell strip pass.
- [design-brief](https://github.com/humbleteam/design-brief) - extract a 5-bullet design brief from messy project inputs, with a gap report for what is missing.

## Who maintains this

[Humbleteam](https://humbleteam.com/) is a digital product design and AI-engineering studio: founded in 2017, working from Prague and Dubai, with 80+ digital awards to the name, including 14 Awwwards wins, a Webby, and a Red Dot. We design digital products for startups and enterprises in fintech, healthtech, sports, and AI, and we build AI infrastructure for design teams - agents, workflows, and skills like this one.

This skill is distilled from the internal playbooks we run on client work: the same checklists behind the case studies at [humbleteam.com/work](https://humbleteam.com/work), for clients like Tinder and Acronis.

- The full 10-skill kit: [Related skills](#related-skills) above, or all repos at [github.com/humbleteam](https://github.com/humbleteam)
- What we do with AI for design teams: [humbleteam.com/ai](https://humbleteam.com/ai)
- Design and AI writing: [humbleteam.com/blog](https://humbleteam.com/blog)
- LinkedIn: [linkedin.com/company/humbleteam](https://www.linkedin.com/company/humbleteam/)
- Talk to us: [hi@humbleteam.com](mailto:hi@humbleteam.com)

Issues and PRs welcome.

MIT - see [LICENSE](LICENSE).
