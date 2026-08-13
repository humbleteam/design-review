# Changelog

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
