# Review rubric

Read this file before scoring anything. It has the 0-4 scoring bands and a compact table of Nielsen's 10 usability heuristics to cite from. Both are referenced from `SKILL.md` step 0.

## Scoring bands (0-4)

Assign exactly one score. The bands describe the artifact as a whole, not any single issue.

| Score | Label | What it means |
|---|---|---|
| 0/4 | Broken | Violates basic accessibility, hierarchy, or trust. The artifact needs a rebuild, not a patch. |
| 1/4 | Significant rework | Five or more heuristic violations. Copy is generic or placeholder. Hierarchy is flat - nothing draws the eye first. |
| 2/4 | Needs work | The structure is sound - the right elements exist in a reasonable order - but the craft is weak: typography, spacing, or contrast issues. |
| 3/4 | Solid, with specific tweaks | Hierarchy and craft are mostly right. 1-3 polish items remain. |
| 4/4 | Ship-ready | Nothing material to fix. Any remaining notes are minor preferences, not defects. |

Score generously when the design serves the project goals the user stated, if they stated any - a screen that looks plain but nails a stated constraint (speed, a technical limitation, a specific user need) should not be penalized for looking plain. Score harshly when the design ignores stated goals outright.

## Nielsen's 10 usability heuristics

A compact citation table. Cite as `Nielsen heuristic #<n> - <name>`. Descriptions below are written for this skill, not quoted from any external source.

| # | Name | What it covers |
|---|---|---|
| 1 | Visibility of system status | The interface keeps people informed with timely, visible feedback about what is happening - loading, saved, in progress, failed. |
| 2 | Match between system and the real world | Words, icons, and flows use language and concepts familiar to the user, not internal system logic or engineering terms. |
| 3 | User control and freedom | People can undo, cancel, or back out of a state they entered by mistake, without being forced down a single path. |
| 4 | Consistency and standards | The same word, icon, or action means the same thing everywhere in the product, and follows the conventions of its platform. |
| 5 | Error prevention | The design removes error-prone conditions before they happen, or asks for confirmation before a destructive action. |
| 6 | Recognition rather than recall | Options and actions stay visible on screen; people choose from what they see instead of having to remember it. |
| 7 | Flexibility and efficiency of use | Shortcuts exist for experienced users without adding clutter for people using the product for the first time. |
| 8 | Aesthetic and minimalist design | Every element on screen earns its place. Extra visual noise competes with the content that actually matters and weakens it. |
| 9 | Help users recognize, diagnose, and recover from errors | Error messages state the problem in plain language and point to a specific fix, not a generic error code. |
| 10 | Help and documentation | When help is necessary, it is easy to search, focused on the task at hand, and does not require reading more than needed. |

## Other citation sources

A citation does not have to be a Nielsen heuristic. These are equally valid, and often more precise for a specific issue:

- **WCAG 2.2 success criteria** - cite by number and short name, e.g. `WCAG 2.2 SC 1.4.3 (contrast minimum)` or `WCAG 2.2 SC 2.5.8 (target size minimum)`. Use these for anything involving contrast, focus order, target size, or assistive technology.
- **Named platform guidelines** - cite the guideline by name and topic, e.g. `Apple Human Interface Guidelines - navigation bars` or `Material Design - elevation`. Use these when the issue is platform-specific (a component that violates iOS or Android conventions).

Do not cite a source you have not actually checked the design against. If none of the above fit an observation cleanly, the observation is probably a taste preference, not one of the 3-6 issues to report.
