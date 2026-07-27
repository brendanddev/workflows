# Enforced Invariants

Project-specific hard invariants go above this line — rules the codebase enforces structurally, worth pinning down explicitly once they're settled. Keep each one concrete: name the type, function, or file that actually enforces it, e.g. "X always returns Y — the only construction path is Z at file:line."

Also use this file for **known, deliberate exceptions** to a stated invariant: document what breaks the rule, why it's justified, and whether it's tracked tech debt or permanent. A silent exception is indistinguishable from a bug; a documented one is a decision.

## Evolved Invariants

Invariants in this section originated as project learnings (discovered during implementation, logged in a skill's `learnings.md`) and were graduated after meeting the bar below. Each entry must reference its source learning.

**Graduation bar:** a learning graduates to a hard invariant here only when it has been observed independently 2+ times, across 2+ separate phases/sessions, and is systemic enough to justify a hard rule rather than a one-off fix. Graduation is *proposed* by `/wrap-up` when a learning meets the bar — it is flagged for explicit human confirmation and never auto-added.

<!-- Entries will be added here via /wrap-up as learnings graduate. -->
