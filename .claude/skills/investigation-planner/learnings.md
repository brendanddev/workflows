# Learnings — investigation-planner

Project-specific observations that improve evidence-first investigation for this project. Keep entries concrete and evidence-based — no generic best practices.

## Format
Each entry: **Date | Phase/Session | Subsystem** / Observation / Evidence / Action-Rule / Impact (High/Med/Low)

## Graduation criteria
An entry graduates to `rules/invariants.md` (`## Evolved Invariants`) when: validated across 2+ phases/sessions, systemic (affects multiple files or a core invariant), and actionable as a hard rule. Graduation is *proposed* by `/wrap-up` — it is never auto-applied. Graduated entries are tagged `[GRADUATED]` and archived here, not deleted.

---

<!--
Template entry — shows the expected shape. Delete once real entries exist,
or keep as a worked example (this one is adapted from a real graduation
case: a pattern flagged 3 times across 2 phases and explicitly held for
human review rather than auto-graduated).
-->

**<YYYY-MM-DD> | <Phase/Session N> | <subsystem>**
**Observation**: A distinct outcome (success/failure/unavailable) was folded into a generic string/text carrier instead of getting its own typed variant, so downstream consumers can only distinguish cases by parsing text.
**Evidence**: `path/to/file.ext:123` — the carrier type/function; commit/PR reference if available.
**Action/Rule**: When a call site needs to distinguish outcomes for different downstream handling, give each outcome its own typed variant/field — never route it through a generic message string as an implicit discriminator.
**Impact**: High — this is the Nth occurrence across M sessions; flag for graduation review per the criteria above, do not graduate unilaterally.
