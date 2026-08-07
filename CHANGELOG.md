# Changelog

## [1.1.0] - 2026-08-07

- Added comparison mode for requests that attach two or more artifacts and ask which one wins. Previously these fell into review mode, which returned a separate critique per artifact and never answered the question. Comparison mode scores each artifact, judges 3-5 shared dimensions in a table, names a winner, and states the one fact that would flip the call.
- Added three edge cases: a comparison where one artifact is unreadable stops instead of guessing, screens compared against a competitor's are judged on the job both do rather than on features only one has, and multiple screens in a single image are only a "which one should I review" question when no comparison was asked for.

## [1.0.0] - 2026-07-12

- Initial release: review mode (screenshot, URL, or HTML snippet in, 0-4 score plus 3-6 Before/After/Why issues out) and advisory mode for open decision questions.
- Added `references/review-rubric.md` with the full scoring bands and a citation table for Nielsen's 10 usability heuristics.
- Documented edge cases: blurry screenshots, unreachable URLs, already-strong artifacts, and vague advisory questions.
