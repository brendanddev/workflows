# Phase N — `<name>` (`<STATUS: PLANNED / ACTIVE / PAUSED / COMPLETE>`)

## Overview
What triggered this phase — a manual testing finding, an audit, a planned capability. Say what shape of problem this phase addresses, not just the topic.

---

## Goal
One or two sentences stating the end condition for this phase — specific enough that "is this phase done" has an obvious answer.

---

## Slice N.1 — `<name>` (`<STATUS>`)

**What changes:**
- Concrete description with `file:line` evidence, not a vague summary. If evidence is missing, that's a sign this slice needs an investigation pass before it's ready to implement.

**Risks:**
- What could go wrong, and what would make you notice.

**Invariants:**
- Any rule this slice must not violate, or is intentionally establishing.

<!--
Optional annotations — use if and when they actually happen, don't force them:

**Decision needed before implementation:** a design fork was found that needs a human call before implementation starts.

**Scope correction found during investigation:** the investigation surfaced that the fix needed to land somewhere different from (or in addition to) where this slice originally assumed. State what changed and why — this is what "surface scope surprises explicitly" looks like in practice.

**Standing pattern flagged, not resolved:** something systemic was noticed but is out of scope for this slice — log it here and in the relevant skill's learnings.md instead of silently expanding scope.
-->

---

## Slice N.2 — `<name>` (`<STATUS>`)

...

---

## Backlog (logged, not slotted as slices)
Findings worth keeping track of but not yet important/urgent enough to schedule:
- `<finding>` — `<why it's backlogged, not fixed>`

---

## Slice Order
N.1 → N.2 → ...

Rationale: state why this order — e.g. mechanical/low-risk fixes first, decision-gated slices after their decision lands, highest-blast-radius slices last so they get full attention rather than being rushed early.

---

## Explicit Non-Goals for Phase N
- What this phase deliberately does not attempt, so scope doesn't silently creep during implementation.

---

## Invariants
Phase-level invariants that every slice in this phase must respect — the throughline, not per-slice detail.
