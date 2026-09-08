# Changelog

## [1.4.0] - 2026-09-08

- Settled the issue count, which shipped as two incompatible ranges. "3-6" stood in the frontmatter description, the step 2c heading, the closing rules line, the rubric twice and four README lines; "2-3" stood in step 2c's own body for a 4/4 screen, in the matching edge-case row and in a README bullet. The two ranges meet only at 3, so a 4/4 review returning two polish items broke the range stated in nine places, and one returning three satisfied both by accident.
- Underneath that sat the larger half: band 3/4 is defined as "1-3 polish items remain", and the band-precedence rule added in 1.3.0 describes a 3/4 as the screen whose fix cost is one to three discrete fixes. The rubric therefore describes screens with one or two real issues, while step 2c set a floor of three for every band except 4/4. On a 3/4 screen with a single polish item the only legal moves were to pad the list to three, which the closing rule forbids by requiring every item to be worth its place, or to break the count. The cap of 6 always carried a reason - an exhaustive list is noise. The floor of 3 carried none.
- Six is now stated as a cap and not a quota, with no minimum count: report every issue that clears the impact bar and nothing else, and let the list run to whatever length the screen earns. Padding a thin list is named as the same failure as listing every flaw, reached from the other side.
- Two floors keep "never an empty critique" intact, and both are now consequences of one rule rather than exceptions with their own numbers. Nothing clears the bar, which is what 4/4 means, so name the 2-3 strongest polish items. Fewer than three clear it on a 0-3 screen, which is normal at 3/4, so list those and say in one line that the list is short because the screen is in good shape - otherwise a two-item review reads as one that stopped early.
- Added an edge case for the short list, rewrote the 4/4 edge-case row to name the impact bar as the reason for its count, and reconciled every statement of the range in `SKILL.md`, `references/review-rubric.md` and the README so all of them agree.

## [1.3.0] - 2026-08-20

- Added a band-precedence rule to `references/review-rubric.md`. Two bands fit most screens - a sound structure with six small heuristic violations matches band 1 by its count and band 2 by its description - and "assign exactly one score" never said which wins. The score is the first line a review prints and the number two runs get compared on, so the same screen could come back 1/4 one day and 2/4 the next without either being wrong.
- The cost of the fix now decides the band: a rebuild (0), a restructure (1), a craft pass over an intact structure (2), one to three discrete fixes (3), nothing material (4). When two bands fit, the one whose fix cost matches the screen wins.
- Stated that the "five or more heuristic violations" in band 1 is evidence, not a threshold. Five minor violations on a structurally sound screen is a 2/4, and severity runs the other way too: one violation that blocks a task, breaks trust, or fails an accessibility floor is enough for 1/4 or 0/4 on its own. The count had no severity weighting and no stated denominator before this.
- Named the denominator: the count is over everything the review found, not over the 3-6 issues it prints. Step 2c is a reporting cap and never a scoring one.
- Step 0, step 2b, the README how-it-works line and the scoring FAQ answer all carry the rule, so every statement of it agrees.

## [1.2.0] - 2026-08-13

- Added grounded advisory mode for a decision question that arrives with an artifact attached ("here is our checkout - should the address form be a modal or a drawer?"). Mode selection routed on the artifact alone, so any attachment forced review mode, and review mode has no slot for an answer: the user got a score and 3-6 Before/After issues, and the closing line is required to name the top fix rather than wrap up, so the question they asked went unanswered. Advisory mode was unreachable because it required no artifact, and the clarify fallback only fires when neither an artifact nor a question is present - leaving no legal move that answers.
- Step 1 is now first-match-wins across four routes, with the definition of a decision question stated: "review this", "is this any good" and "what's wrong with this" ask about the artifact's own quality and stay in review mode.
- Grounded advisory keeps the advisory shape and raises the evidence bar: claims the artifact can settle rest on an observable fact from it, the single fact that carried the recommendation is named, anything the artifact cannot settle is labeled as general guidance, and at most three fixes follow - only ones bearing on the decision.
- Added three edge cases (artifact plus decision question, a question whose subject is not visible in the artifact, and a quality question that stays a review), a worked example in the README, and matching updates to the mode-detection and FAQ sections so every statement of the rule agrees.

## [1.1.0] - 2026-08-07

- Added comparison mode for requests that attach two or more artifacts and ask which one wins. Previously these fell into review mode, which returned a separate critique per artifact and never answered the question. Comparison mode scores each artifact, judges 3-5 shared dimensions in a table, names a winner, and states the one fact that would flip the call.
- Added three edge cases: a comparison where one artifact is unreadable stops instead of guessing, screens compared against a competitor's are judged on the job both do rather than on features only one has, and multiple screens in a single image are only a "which one should I review" question when no comparison was asked for.

## [1.0.0] - 2026-07-12

- Initial release: review mode (screenshot, URL, or HTML snippet in, 0-4 score plus 3-6 Before/After/Why issues out) and advisory mode for open decision questions.
- Added `references/review-rubric.md` with the full scoring bands and a citation table for Nielsen's 10 usability heuristics.
- Documented edge cases: blurry screenshots, unreachable URLs, already-strong artifacts, and vague advisory questions.
