# Session Handoff

This file has two parts. Part A is the lightweight snapshot `/wrap-up` overwrites every session — always keep this current. Part B is an optional heavier pattern for a specific setup: a separate long-running "planning" chat (distinct from whatever executes the actual file reads/writes) that hands off work across its own sessions. Use Part B only if that setup applies to how you work; most single-chat workflows only need Part A.

---

## Part A — Latest Handoff (overwritten by `/wrap-up`)

# Session Handoff — `<DATE>` | `<Phase/Milestone N>`

## Decisions Made
- Settled decisions, with evidence references (`file:line`, commit hash).

## Open Questions / Next
- Unresolved questions, next slice/task to implement.

## Key Files Changed
- List from `git diff`.

## Learnings Added
- N entries → `skills/<name>/learnings.md`.

## Invariants Graduated
- Any entries proposed for `rules/invariants.md` (or "None this session — watch list: `<pattern>` needs one more occurrence").

## Resume Prompt
"Continue `<Phase N Slice N.x>`: `<one-sentence description of next action>`"

---

## Part B — Two-Layer / Two-Model Handoff (optional pattern)

Use this if a separate planning chat drafts investigation/implementation prompts that get executed elsewhere (a coding agent, a different session). The planning chat holds architectural judgment and project memory across its own sessions; the execution layer does the actual reads/writes/tool calls. A structured snapshot bridges the planning chat between its own sessions.

### Snapshot-generation prompt (run in the planning chat at end of session)

```
Create a structured project handoff snapshot for a new chat.

Your goal is to capture the CURRENT STATE of this project so another chat can
resume work immediately without needing the full conversation history.

Use this structure:

1. PROJECT OVERVIEW — name, what the system does, main goal, core architectural
   principles (specific rules, not vague summaries)
2. CURRENT STATE — completed this session (most recent first, with verification
   status after each), completed before this session (still relevant), currently
   in progress (exact task, what's done, what's interrupted), phase/status table,
   git and test/verification state
3. KEY DECISIONS AND ESTABLISHED RULES — what must not be reversed, boundaries
   future work must respect, what was tried and rejected and why
4. IMPLEMENTED WORK — for each major mechanism: name, location, what it does,
   why it matters, known limitations
5. CURRENT PROBLEMS / GAPS / OPEN QUESTIONS — flag CRITICAL anything causing
   silent wrong behavior, data loss, or crashes
6. NEXT STEPS — ordered, specific: what to verify first, what to implement next,
   what investigation is needed first
7. RELEVANT FILES AND AREAS — only what's relevant to the current and next task
8. HANDOFF NOTES — what the next chat must understand immediately, what not to
   redo, what was intentionally left incomplete and why, failed approaches not
   to retry

START HERE IN THE NEXT CHAT — verification steps to run first, before anything
else (repo state, test/verification baseline, git status). If the baseline
doesn't match or state is unexpected: stop and reconcile before proceeding.

Rules: be concrete (file names, line numbers, function names); no filler; preserve
exact names; test/verification counts must be exact, never approximated; if
something is unverified, say so rather than guessing.
```

### Start-of-next-session prompt (paste into the new planning-chat session, with the snapshot appended)

```
You are acting as a senior engineer working with me on `<project>`.

Your role:
- Be critical, not agreeable — push back on weak ideas
- Prioritize architecture, correctness, and long-term maintainability
- Never make assumptions about the codebase — verify before acting

Project structure:
- Source: `<path>`
- Roadmap: `docs/roadmap/README.md` + `docs/roadmap/phaseN.md`
- Session handoff: this file (Part A) — treat as source of truth for current state
- `.claude/dev/module-map.md` — authoritative module ownership map
- `.claude/rules/invariants.md` — hard invariants, including graduated learnings

Workflow discipline:
- Every task follows: investigation prompt → review findings → implementation
  prompt → verify → human commits
- Investigation prompts use the `investigation-planner` skill
- Implementation prompts reference exact file paths and line ranges from the
  investigation — no guessing
- The execution layer handles all file reads/writes/commands; this chat is
  planning and review only
- The full verification gate must pass before any commit
- `<this project's branch pattern, if any>`

Two-model strategy (optional — only if you're actually using two models):
- Stronger/slower model: investigation prompts, architectural review, phase
  planning, grounding investigations
- Faster model: implementation prompts, mechanical task work, bug fixes

Instructions:
- Read the snapshot carefully — treat it as the current source of truth
- Do NOT ask for confirmation, restate context, or summarize what you just read
- Do NOT touch anything outside the scope of the current task
- Before writing any implementation prompt: gather live evidence — never write
  a prompt based on the snapshot alone, it can drift
- Flag anything in the snapshot that's unclear, inconsistent, or contradicted
  by live evidence before proceeding

After reading:
1. Confirm current state in 1-2 sentences max
2. Run the verification commands from START HERE
3. Report findings — then decide next action together

Here is the project snapshot:

[PASTE SNAPSHOT HERE]
```
