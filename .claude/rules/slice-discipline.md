# Slice / Change Discipline

A "slice" is one bounded, independently-verifiable unit of change — small enough to review in one pass, large enough to be a complete behavior.

## The Pattern (follow exactly)
1. Identify the exact failure mode or requirement — reproduce the bug, or write the failing test, first.
2. Find the code location that owns the decision — search before assuming; see `investigation-planner`.
3. Make the minimal change — the smallest edit that fixes the identified cause or implements the identified behavior.
4. Add a test that would have caught the regression, not just a test that exercises the new code path.
5. Run the full verification gate — this is the hard stop. See `WORKFLOW.md` for what the gate must include.
6. Report to the human — never commit. The human reviews and commits manually.

## Where Changes Live
Restate this project's own layer boundaries here (copy from `.claude/rules/architecture.md` and `.claude/dev/module-map.md` once populated), e.g.: "business logic lives only in `<layer>`; `<interface layer>` contains no business logic — dispatch and rendering only."

## State Discipline
Generalized rule: any new mutable state field must be explicitly initialized (or reset) at *every* code path that creates or resets fresh state — a partially-initialized state object is a silent, hard-to-trace bug source. Name this project's actual constructor/reset function(s) here once known, so this rule has a concrete anchor instead of being abstract.

## Test Rules
- Integration tests live at: `<path>`
- Unit tests live at: `<convention, e.g. inline test module next to the code>`
- One test per behavioral change, minimum.
- A test that would not have caught the original bug/regression is not the right test for this change.

## Commit Rules
- Never make commits — the human always commits manually.
- One behavioral change + one test per commit, where practical.
- Commit message format: `<this project's convention, e.g. type(scope): description>`
