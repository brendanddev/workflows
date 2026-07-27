# WORKFLOW

This is the discipline this repo packages, written as onboarding for a human + AI pair working on any codebase. Read this file once, fully, before using anything else here — the `.claude/` and `docs/` contents are the mechanism; this file is the reasoning behind them.

It was extracted from a long real engagement (50+ phases on a Rust project) that used this loop successfully, plus one deliberate improvement (investigation-report persistence) that engagement did *not* have. Where a rule below is a direct, confirmed practice from that engagement, it's stated plainly. Where it's a recommended addition that wasn't literally observed, it's marked as such — don't mistake "sounds like good practice" for "proven."

---

## The core loop

**Investigation → Implementation → Verify → Learnings.**

Every non-trivial change goes through all four steps, in order, without skipping. Skipping investigation is the single most common way this discipline breaks down — a prompt that "seems simple enough to just implement" is exactly the kind that turns out to have a wrong assumption baked in.

### 1. Investigation (read-only)

- Investigation gathers evidence. It does not write code, and it does not propose a final decision beyond a ranked plan. Use the `investigation-planner` skill (`.claude/skills/investigation-planner/SKILL.md`).
- Every claim in an investigation report cites exact `file:line` evidence — never "the config handling" or "somewhere in the parser," always `src/foo.rs:142`. Vague descriptions in an implementation prompt are a sign the investigation wasn't finished.
- If a previously-cited `file:line` reference has drifted (the file moved, line numbers shifted since it was written), **re-investigate that reference** — do not guess that it's "probably still right." Stale citations that go unchecked are how implementation prompts silently drift from reality.
- Investigation and implementation are separable roles/prompts. This separation is valuable even with a single model, because it forces the "what does the evidence say" step to complete before the "what should I do about it" step starts. It becomes more valuable if you use two different models for the two roles — a stronger model for investigation and architectural judgment, a faster one for mechanical implementation — since investigation quality gates everything downstream. See `docs/session-handoff-template.md` (Part B) for a concrete two-model, two-chat-layer pattern.

### 2. Investigation reports are files, not just chat

Save non-trivial investigation reports to `docs/investigations/` (see that directory's `README.md` and `investigation-template.md`), instead of leaving them only in the conversation that produced them.

This is a deliberate addition, not something the source engagement proved out — that engagement kept investigation detail in ephemeral prompts and only persisted a compressed session-handoff summary at the end. The gap that leaves: a report's full evidence trail disappears once its chat ends, and the *next* session (or a different model doing the implementation) has to either re-derive it or trust a lossy summary. Saving the report itself closes that gap. Skip this for trivial investigations consumed immediately in the same session — save it when the report needs to outlive the current conversation.

### 3. Scope surprises get surfaced, not silently resolved

Investigation sometimes turns up more than what was asked for: an adjacent bug, a second bypass of the same guard, a security-relevant issue nobody scoped in. When that happens:

- **Never silently expand scope** to fix it as part of the current task.
- **Never silently drop it** because it's inconvenient to mention.
- Surface it explicitly, as its own item, for a human decision: fix now (as a separate slice), backlog it, or spin off a dedicated investigation.

This is a confirmed practice, not a nice-to-have — real phase docs from the source engagement have explicit "scope correction found during investigation" and "decision needed before implementation" sections precisely for this, and a "standing pattern flagged, not resolved" section for systemic findings that are correctly out of scope for the task at hand. `docs/roadmap/phaseN-template.md` includes the same annotations as optional sections — use them when they apply, don't force them when they don't.

### 4. Implementation

- Implementation prompts are written from the investigation report's evidence, not from memory of the codebase or a guess at what "should" be there.
- Make the minimal change that addresses the identified cause — see `.claude/rules/slice-discipline.md`.
- Follow this project's own safe-modification checklists (`.claude/rules/safe-modification.md`) for recurring change categories (new capability, changing a core pathway, changing a mutating pathway). The one universal principle worth internalizing from that file: **revalidate at execution time, not proposal time.** Any approval-gated or previously-validated action must re-check its own preconditions immediately before it executes, not just when it was proposed — state can go stale in the gap between "this was checked" and "this actually runs." That's a general security/correctness principle for any propose-then-execute system, not specific to any one language or stack.

### 5. Tests that prove the fix, not just exercise it

For any security- or correctness-relevant fix: the test must reproduce the *original* failure and prove it's now blocked — not merely get a green checkmark on the new code path.

- Confirmed practice: "a test that wouldn't have caught the original bug is not the right test" (`.claude/rules/slice-discipline.md`).
- Recommended, not confirmed as prior practice: **mutation-test your own test** — temporarily revert the fix and confirm the test actually fails. A test that passes whether or not the fix is present isn't testing anything. This wasn't a literal documented practice in the source engagement, but it's a natural, cheap extension of "the test must be the regression catch" and is worth doing for anything security-relevant.

### 6. Verification gate — no exceptions

A single verification gate (this project's equivalent of format + typecheck + lint + full test suite) gates every commit. No exceptions, no "just this once," no skipping it because the change looks small. Confirmed practice — the source engagement's hard stop was "run the full test suite; it must pass," stated identically at every slice.

### 7. Nothing is committed automatically

The human reviews and commits. Every time. This is a repeated, explicit rule in the source material (not an inferred convention) — the AI reports what's ready; a person decides when and how it becomes a commit.

### 8. Comments: why, not what

New functions get comments that explain *why* — a non-obvious constraint, a workaround, a decision that would otherwise look arbitrary — never comments that restate *what* the code visibly does. Note: this convention wasn't found written down anywhere in the source engagement's actual files; it's included here as a reasonable general convention, not a confirmed practice from that project.

### 9. Learnings → graduation → invariants

- During implementation, record project-specific observations in the relevant skill's `learnings.md` — see the format and graduation criteria in `.claude/skills/investigation-planner/learnings.md`.
- An observation graduates from "learning" to hard "invariant" (`.claude/rules/invariants.md`, `## Evolved Invariants`) only after **2+ independent occurrences across 2+ separate phases/sessions**, and only when it's systemic enough to state as a rule. This bar is deliberately strict — most one-off observations should stay as learnings, not invariants.
- Graduation is *proposed*, by `/wrap-up` (`.claude/commands/wrap-up.md`), never auto-applied. A learning that clearly meets the bar still gets flagged for explicit human confirmation before it becomes a hard rule. This is a confirmed practice with a real example: a pattern (a generic string carrier used as an implicit success/failure discriminator) was independently observed three times across two phases, explicitly flagged in both the skill's `learnings.md` and the phase doc as meeting the graduation bar — and deliberately left ungraduated pending human review, rather than promoted automatically. See the worked example left in `.claude/skills/investigation-planner/learnings.md`.

### 10. Known exceptions are documented, not hidden

When a change deliberately violates a stated invariant or architecture rule (and sometimes it's the right call), document it inline where the violation happens and in `.claude/rules/architecture.md` / `invariants.md` — what breaks the rule, why it's justified, whether it's tracked tech debt or permanent. A silent exception is indistinguishable from a bug; a documented one is a decision someone can revisit.

### 11. Periodic independent audits

At natural phase/milestone boundaries, run an independent audit of the codebase — not just the code just touched. Two things matter here, both confirmed practice:

- **Tell the auditor what's already fixed.** An audit that doesn't know what's already been addressed re-finds the same issues and wastes the pass. Give it the list of resolved findings so it explores untouched territory instead.
- **Use more than one audit tool/pass where practical.** Different tools and different models catch different classes of issues — the source engagement's real phase-50 audit combined a strong-model architectural review with a separate tool's cross-cutting pass, and the two found different, complementary things.

---

## How the pieces fit together

| Piece | Purpose |
|---|---|
| `.claude/skills/investigation-planner/` | Read-only evidence-gathering before any implementation prompt |
| `.claude/skills/debug-investigation/` | Structured root-cause narrowing for recurring failure classes (fill in as they're discovered) |
| `.claude/skills/usage-analyzer/` | Meta-skill: audits token usage of this very `.claude/` setup |
| `.claude/rules/invariants.md` | Hard rules the codebase enforces structurally, plus graduated learnings |
| `.claude/rules/safe-modification.md` | Concrete checklists for recurring change categories |
| `.claude/rules/slice-discipline.md` | The mechanical loop: minimal change, test, verify, human commits |
| `.claude/rules/architecture.md` | Layer boundaries and documented exceptions |
| `.claude/commands/wrap-up.md` | End-of-session: extract learnings, propose graduations, update handoff, sync check |
| `.claude/dev/module-map.md` | Living ownership map — what each module owns and must not do |
| `docs/roadmap/` | Phase-level planning: goal, slices, risks, invariants, explicit non-goals |
| `docs/investigations/` | Persisted investigation reports (the one deliberate addition beyond the source engagement) |
| `docs/session-handoff-template.md` | Session-to-session continuity, lightweight (Part A) or two-layer (Part B) |

Populate `.claude/dev/module-map.md`, `.claude/rules/architecture.md`, and `docs/roadmap/README.md` early — they're empty templates on purpose, but the whole loop above is much weaker without them, because investigation and safe-modification both lean on having a real map to search against.
