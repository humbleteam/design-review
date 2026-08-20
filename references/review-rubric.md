# Review rubric

Read this file before scoring anything. It has the 0-4 scoring bands, the rule for picking one band when two of them fit, and a compact table of Nielsen's 10 usability heuristics to cite from. All three are referenced from `SKILL.md` step 0.

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

### Choosing one band when two of them fit

Two bands describe the same screen more often than not. A screen with a sound structure and six small heuristic violations matches band 1 by its count and band 2 by its description, and "assign exactly one score" does not say which wins. The score is the first thing a review prints and the thing two runs get compared on, so an unsettled tie is how the same screen comes back 1/4 on Monday and 2/4 on Friday. Two rules settle it.

**The cost of the fix decides the band. The number of violations does not.** Read the bands as a ladder of what it would take to make the screen shippable: a rebuild (0), a restructure that keeps the idea and little else (1), the same structure with the craft redone (2), one to three discrete fixes (3), nothing material (4). When two bands fit, the one whose fix cost matches the screen wins.

**A count is evidence, not a threshold.** The "five or more" in band 1 describes what a flat hierarchy and placeholder copy usually look like once counted. It does not promote a screen on its own: five minor violations on a structurally sound screen is a 2/4. Severity runs the other way too - one violation that blocks a task, breaks trust, or fails an accessibility floor is enough for 1/4 or 0/4 by itself.

Count over everything the review found, not over the issues it reports. The 3-6 cap in `SKILL.md` step 2c is a reporting cap: it decides what gets printed, never what the screen scores.

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
