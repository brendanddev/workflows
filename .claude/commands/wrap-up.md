# /wrap-up

End-of-session summary, learnings extraction, graduation candidates, and handoff snapshot. Run at the end of any implementation session. Proposes all changes — writes nothing without explicit human approval.

## Steps

**1. Summarize session**
- Run `!git log --oneline -5` and `!git diff --name-only HEAD~3..HEAD` (adjust the range to what actually happened this session)
- Identify touched subsystems/modules (match changed files to `.claude/dev/module-map.md` ownership)
- Identify which skills were invoked this session (`investigation-planner`, `debug-investigation`, etc.)

**2. Extract learnings**
For each touched subsystem or invoked skill, propose 1-3 new entries in exact `learnings.md` format:
- Date | Phase/Session | Subsystem
- Observation (one sentence, specific to this project)
- Evidence (file path, commit, or reproduction)
- Action/Rule (actionable for future sessions)
- Impact (High/Med/Low)

Criteria — an entry is worth adding only if it is:
- Project-specific (not a generic language/framework best practice)
- Evidence-based (grounded in a specific file, bug, or commit)
- Not already captured in `rules/` or `dev/`
- Actionable for future investigations or debugging

**3. Graduation check**
Scan `skills/*/learnings.md` for entries tagged High impact that appear in 2+ phases/sessions (search by subsystem keyword or cross-reference git history). An entry graduates to `rules/invariants.md` under `## Evolved Invariants` when:
- Validated across 2+ phases/sessions with no violations
- Systemic (affects multiple files or a core invariant)
- Actionable as a hard rule

Propose graduated entries with the original learning referenced. Tag the original entry `[GRADUATED]` rather than deleting it. **Flag for human review — do not decide graduation unilaterally, even if the entry clearly meets the bar.**

**4. Sync check**
Check whether any of this project's own "living" reference docs have gone stale relative to what actually happened this session — e.g.:
- A test-count or build-status baseline recorded in a CLAUDE.md / README
- `dev/module-map.md` entries for newly added or moved modules
- Any `invariants.md` line-number references that shifted

Propose specific updates; do not assume these are current without checking.

**5. Generate handoff snapshot**
Propose content for `docs/session-handoff-template.md`'s "latest" section (overwrite), following its structure: Decisions Made / Open Questions & Next / Key Files Changed / Learnings Added / Invariants Graduated / Resume Prompt.

**6. Output and approval**
Show all proposed changes:
- New learnings entries (with target file)
- Any graduation proposals (with target section in `invariants.md`)
- Handoff snapshot content
- Any sync updates

Do not write any file until the human explicitly approves. State clearly: "Ready to write — approve to proceed."

## Constraints
- Evidence-based only — no generic best practices, no hallucinated file paths
- Never write without explicit human approval
- Keep learnings entries to 4-8 lines max
- Keep the handoff snapshot under 200 lines — reference other docs rather than duplicating them
- Do not touch source files — this command only writes to `.claude/` and `docs/`
