---
name: design-review
description: Run a structured UX critique on a screenshot, URL, or HTML snippet - a 0-4 score, 3-6 prioritized issues each with Before/After/Why, and one citation per claim. Given two or more variants, compares them on shared dimensions and names a winner. Given a screen plus a decision question, answers the question with the screen as evidence instead of returning a critique. Trigger phrases - "review this design", "critique this screen", "what's wrong with this UI", "score this design", "which of these two is better", "did the redesign improve this", "should I use a modal or a drawer". Do not use for WCAG-only accessibility audits (use accessibility-audit) or turning a mockup into a dev spec (use design-handoff).
---

# Design review

Critique a UI artifact or answer a design decision question, always with a citation behind every claim.

## Step 0 - load the rubric

Before writing any critique, read [references/review-rubric.md](references/review-rubric.md) if it is not already in context. It has the 0-4 scoring bands, the rule that picks one band when two of them describe the same screen, and the compact table of Nielsen's 10 usability heuristics you cite from. Do not skip this even if you know the heuristics - the exact wording of the bands matters for consistent scoring across runs, and the tie-break is what keeps two runs on one screen from returning two different numbers.

## Step 1 - pick a mode

Two facts settle it. Is an **artifact** attached - a pasted screenshot, an image, a URL, or an HTML snippet in a code block? And does the message ask a **decision question** - a choice between patterns, components, or flows ("should I use X or Y", "how should I handle...")? "Review this", "is this any good" and "what's wrong with this" are not decision questions: they ask about the artifact's own quality, which is what review mode answers.

Take the first match:

1. **Comparison mode**: two or more artifacts, and the question asks which of them wins ("A or B", "which of these is better", "did the redesign improve this").
2. **Grounded advisory mode**: at least one artifact, and a decision question the artifact does not itself answer ("here is our checkout - should the address form be a modal or a drawer?"). The question governs the mode; the artifact is evidence, not the subject.
3. **Review mode**: one artifact, no decision question.
4. **Advisory mode**: no artifact, a decision question.

Two or more artifacts and no decision question is two separate reviews, not a comparison - ask which the user wants before starting. If the request has neither an artifact nor a decision question, ask what the user wants reviewed instead of guessing.

An attached artifact never silently downgrades a question into a critique. Answering a decision question with a full review is the same failure as answering a comparison with two critiques: the output is well formed, and the question the user asked is still open.

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

More than one band will fit most screens. When two do, the cost of the fix decides which one wins - a rebuild, a restructure, a craft pass, 1-3 discrete fixes, or nothing material - and the violation count in band 1 is evidence of a structural problem, never a threshold that promotes a screen on its own. Five minor violations on a sound structure is a 2/4; one violation that blocks a task or fails an accessibility floor can be a 1/4 or 0/4 by itself. Count over what the review found, not over the 3-6 issues it reports - the cap in 2c decides what gets printed, not what the screen scores. Full rule in `references/review-rubric.md`.

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

## Step 5 - grounded advisory mode

A decision question with an artifact attached. The answer keeps the advisory shape; the artifact raises the evidence bar, it does not change what is being asked.

- Answer the question first, in one sentence. The critique, if there is one at all, comes after.
- Every claim the artifact can settle rests on an observable fact from it, to the same standard as `Before` in review mode: "the address form is nine fields deep and the page already carries a sticky order summary", not "the form looks long". Claims the artifact cannot settle - traffic, device mix, what users do next - are still allowed as general guidance, but say which they are rather than dressing them as observations.
- Name the one observable fact that carried the recommendation. If the artifact flips the answer you would have given without it, say so plainly: that is the entire value of having it attached.
- If the artifact does not show the element the question is about, say so in one line and answer from general guidance. Never infer the missing element from the surrounding layout.
- Close with at most 3 review-mode issues, and only ones that bear on the decision. Everything else waits for a full review - offer it in the closing line rather than smuggling an unrequested critique in under the answer.

### Output format - grounded advisory mode

```
**Recommendation:** <one-sentence direct answer to the question that was asked>

- <Claim 1, resting on an observable fact from the artifact>. Why: <citation> - <one-phrase rationale>.
- <Claim 2>. Why: <citation> - <one-phrase rationale>.
- <Claim 3>. Why: <citation> - <one-phrase rationale>.

**What in the artifact decided it:** <the single observable fact that carried the recommendation>

**Also worth fixing:** <0-3 issues in Before/After/Why, only ones that bear on this decision - or "Nothing that bears on this decision. Ask for a full review if you want the rest.">
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
| An artifact is attached and the question is a decision question ("here is the screen - modal or drawer?") | Grounded advisory mode (Step 5). Answer the question first, with the artifact as evidence. A full review the user did not ask for leaves the question open, however well formed it is. |
| A decision question whose subject is not visible in the attached artifact (asked about the empty state, sent the filled one) | Say in one line that the artifact does not show it, then answer from general guidance and cite as usual. Do not infer the missing element from the surrounding layout. |
| An artifact plus "is this any good?" or "what's wrong with this" | Review mode. These ask about the artifact's own quality rather than for a choice between options, so they are not decision questions. |

## Rules that hold in every mode

- One citation per issue or claim. Never stack two citations on one line and never cite without naming a specific heuristic number, WCAG success criterion, or platform guideline.
- Every `After` (review mode), verdict (comparison mode), or claim (advisory and grounded advisory modes) must be concrete enough to hand to a developer or designer with no follow-up question.
- Never invent a source, a study, or a statistic. If you are not sure a claim is grounded, cut the claim.
- Keep the tone direct and factual. No hedging language like "this might possibly be an issue" - either it is an issue worth the 3-6 slot or it is not.
