---
name: design-review
description: Run a structured UX critique on a screenshot, URL, or HTML snippet - a 0-4 score, 3-6 prioritized issues each with Before/After/Why, and one citation per claim. Given two or more variants, compares them on shared dimensions and names a winner. Trigger phrases - "review this design", "critique this screen", "what's wrong with this UI", "score this design", "which of these two is better", "did the redesign improve this", "should I use a modal or a drawer". Do not use for WCAG-only accessibility audits (use accessibility-audit) or turning a mockup into a dev spec (use design-handoff).
---

# Design review

Critique a UI artifact or answer a design decision question, always with a citation behind every claim.

## Step 0 - load the rubric

Before writing any critique, read [references/review-rubric.md](references/review-rubric.md) if it is not already in context. It has the 0-4 scoring bands and the compact table of Nielsen's 10 usability heuristics you cite from. Do not skip this even if you know the heuristics - the exact wording of the bands matters for consistent scoring across runs.

## Step 1 - pick a mode

- **Review mode**: one artifact is present in the request - a pasted screenshot, an image, a URL, or an HTML snippet in a code block.
- **Comparison mode**: two or more artifacts are present and the request asks which one wins ("A or B", "which of these is better", "did the redesign improve this"). Two artifacts and no decision question is two separate reviews, not a comparison - ask which the user wants before starting.
- **Advisory mode**: no artifact, and the message is a decision-shaped question ("should I use X or Y", "how should I handle...").

If the request has neither an artifact nor a decision question, ask what the user wants reviewed instead of guessing.

## Step 2 - review mode

### 2a. Handle the artifact

- Screenshot or image: read it directly.
- URL: fetch and render it if you have that capability; otherwise ask the user to paste a screenshot.
- HTML snippet: read the markup and inline styles as given.

### 2b. Score first, 0-4

Assign one score before listing issues. Use these bands exactly:

- **0/4 - broken**: violates basic accessibility, hierarchy, or trust. Needs a rebuild, not a patch.
- **1/4 - significant rework**: five or more heuristic violations, generic or placeholder copy, flat hierarchy with no clear focal point.
- **2/4 - needs work**: the structure is sound but the craft is weak - typography, spacing, or contrast issues.
- **3/4 - solid, with specific tweaks**: hierarchy and craft are mostly right, 1-3 polish items remain.
- **4/4 - ship-ready**: nothing material to fix, only minor preferences.

Score generously when the design serves the project goals the user stated, if they stated any. Score harshly when it ignores them.

### 2c. Pick 3-6 issues, ranked by impact

Never list every flaw you notice - an exhaustive list is noise and noise is a failure mode of this skill. Rank by impact and stop at 6, even if more issues exist. If the artifact scores 4/4, still list 2-3 items, framed as polish, not blockers.

For each issue, write exactly three lines:

- **Before**: a specific, observable fact. Not "the layout feels cluttered" - "12 UI elements sit inside a single 320px-wide card with no grouping."
- **After**: a fix a person could ship in under an hour. Not "improve the hierarchy" - "move the secondary actions into an overflow menu and keep only the primary action visible."
- **Why**: exactly one citation - a Nielsen heuristic by number and name, a WCAG 2.2 success criterion by number, or a named platform guideline (Apple Human Interface Guidelines, Material Design). One phrase of rationale after the citation, then stop. Do not lecture.

### 2d. Output format - review mode

```
## <Artifact name, 2-4 words> - score <X>/4

### 1. <Short issue title>
- **Before:** <observable fact>
- **After:** <fix, doable in under an hour>
- **Why:** <citation> - <one-phrase rationale>

### 2. <Short issue title>
- **Before:** <...>
- **After:** <...>
- **Why:** <citation> - <...>

...

**Fix this first:** <one paragraph naming the single most important fix and why it outranks the others>
```

Number issues in the order you want them fixed, most impactful first. The closing line always names the single highest-priority fix - never a generic wrap-up.

## Step 3 - comparison mode

Two or more artifacts, and the question is which one wins. Reviewing each one in full is the failure mode here: it returns two critiques and still no answer.

### 3a. Score each artifact

Use the bands from Step 2b, one score per artifact. The scores are a summary, not the argument - two variants can both score 3/4 and still have a clear winner.

### 3b. Compare on shared dimensions

Pick 3-5 dimensions that matter for the job these screens do - for example hierarchy, clarity of the primary action, scan cost, accessibility, information density. Judge every artifact against every dimension, and keep the wording observable, the same standard as `Before` in review mode. A point that applies to only one artifact is a review note, not a comparison line - hold it for the closing "Worth fixing in the winner" list.

### 3c. Name a winner

Always name one, even when the margin is small. A comparison that ends in "it depends" has not done the job. State what would change the call - the one fact about users, goals, or constraints that would flip it. If the honest answer is that the strongest screen takes parts from both, say which parts and from which variant.

### 3d. Output format - comparison mode

```
## <A> vs <B> - <A> <X>/4, <B> <Y>/4

| Dimension | <A> | <B> | Edge |
|---|---|---|---|
| <dimension> | <observable fact> | <observable fact> | <A or B> |
| <dimension> | <observable fact> | <observable fact> | <A or B> |
| <dimension> | <observable fact> | <observable fact> | <A or B> |

**Winner: <A or B>.** <One paragraph naming the dimension that decided it, with exactly one citation - same citation rules as review mode.>

**What would change the call:** <the one fact that would flip the decision>

**Worth fixing in the winner:** <1-3 issues, Before/After/Why, only if they survive the impact bar from Step 2c>
```

Every artifact keeps its own column for its whole life in the table - never merge two variants into one "both" cell, because the point of the table is that the eye can run down one column.

## Step 4 - advisory mode

No artifact, a decision question instead. Skip the Before/After/Why structure entirely.

- Give one direct recommendation in a single sentence.
- Follow with 3 bullets (5 is the hard cap), each one claim plus one citation, same citation rules as review mode.
- If the question is open enough that any reasonable answer would need more context (no options listed, no context on the users or the constraint), ask one targeted clarifying question instead of guessing. Surface 2-3 likely interpretations so the user can just pick one.

### Output format - advisory mode

```
**Recommendation:** <one-sentence direct answer>

- <Claim 1>. Why: <citation> - <one-phrase rationale>.
- <Claim 2>. Why: <citation> - <one-phrase rationale>.
- <Claim 3>. Why: <citation> - <one-phrase rationale>.

If you want specifics, share a screenshot or a URL and I'll do a full review.
```

## Edge cases

| Situation | What to do |
|---|---|
| Screenshot is blurry or too low-res to read text or spacing | Say so directly and ask for a higher-resolution image or a URL. Do not guess at what a blurry element says. |
| URL is unreachable (auth wall, 404, requires login) | Say the URL could not be opened and ask for a screenshot or an HTML export instead. |
| Artifact is already strong (would score 4/4) | Still produce the full numbered list - 2-3 items, framed as polish/nice-to-have, not as blockers. Never return an empty critique. |
| No stated project goals | Review against the general heuristics and guidelines in `references/review-rubric.md` alone. Do not invent goals or a target audience. |
| Advisory question too vague ("which is better?" with no options named) | Ask one clarifying question that lists 2-3 likely options rather than guessing which one the user means. |
| Multiple distinct screens in one screenshot, no comparison asked for | Ask which one to review, or offer to review each separately if the user wants both. Two variants of the same screen with a "which is better" attached is comparison mode instead - do not ask the user to pick one for you. |
| One artifact in a comparison is unreadable or unreachable | Do not compare. Say which one failed and ask for a replacement - a comparison where half the evidence is a guess is worse than no comparison. |
| The artifacts in a comparison are different screens, not variants of one (our pricing page vs a competitor's) | Comparison mode still applies, but the dimensions must be about the job both screens do, not about features only one of them has. Say so in one line before the table. |

## Rules that hold in every mode

- One citation per issue or claim. Never stack two citations on one line and never cite without naming a specific heuristic number, WCAG success criterion, or platform guideline.
- Every `After` (review mode), verdict (comparison mode), or claim (advisory mode) must be concrete enough to hand to a developer or designer with no follow-up question.
- Never invent a source, a study, or a statistic. If you are not sure a claim is grounded, cut the claim.
- Keep the tone direct and factual. No hedging language like "this might possibly be an issue" - either it is an issue worth the 3-6 slot or it is not.
