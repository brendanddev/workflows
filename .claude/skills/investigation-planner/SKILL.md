---
name: investigation-planner
description: Evidence-first codebase exploration before implementing any feature, fix, or change. Use before writing any implementation prompt. Produces exact file paths, line numbers, signatures, and a ranked implementation plan grounded in live evidence — never assumptions.
---

## Reference Materials

**Project learnings** (review before starting — project-specific gotchas and patterns):
!cat .claude/skills/investigation-planner/learnings.md

---

You are the investigation phase of this project's development workflow. Your job is to gather all evidence needed to write a precise implementation prompt. You do not write code. You do not modify files. You report findings only.

## When to use
Before any non-trivial change — new features, new endpoints/commands, behavioral changes, bug fixes. Skip only for genuinely mechanical one-liners with no ambiguity.

## Workflow

### Step 1 — Understand the goal
State the exact change in one sentence. Identify the change category (adjust to this project's own shape — e.g. "new tool," "new endpoint," "config change," "bug fix") and which subsystems it touches.

### Step 2 — Find the reference implementation
Almost every change has a prior example somewhere in the codebase. Find the closest one and show its exact file path and line numbers — don't describe it from memory, grep for it.

### Step 3 — Map all touch points
For each file that needs changing, show:
- The exact line range to modify
- The type or function signature involved
- Whether any exhaustive match/switch/enum will break (does adding a case anywhere require touching other call sites?)

Use targeted search over full reads:
```bash
grep -n "pattern" file
sed -n 'X,Yp' file
grep -rn "pattern" src/
grep -n -A 10 "fn name" file
wc -l file          # check size before reading a whole file
```

### Step 4 — Identify risks and gaps
- Exhaustive matches/switches that will break — list every one
- Invariants from `.claude/rules/invariants.md` that apply
- Tests that need updating
- Anything in the reference implementation that doesn't apply to this change

### Step 5 — Produce the findings report
Output exactly:

**Reference implementation:** `file:line` — what it does

**Touch points:**
| File | Line range | What changes |
|------|-----------|--------------|
| ... | ... | ... |

**Exhaustive matches that break:**
- List each one

**Risks:**
- List each one

**Recommended implementation order:**
1. Step one
2. Step two
...

**Do not proceed past this point.** The findings report is the output. Implementation happens in a separate prompt — see `WORKFLOW.md`.

If this report is meant to be reusable beyond the current chat (a second AI instance implements it, or a human picks it up later), save it as a file under `docs/investigations/` instead of leaving it only in this conversation. See `docs/investigations/README.md`.

## Constraints
- Never read a full file if a targeted grep can answer the question
- Never assume a line number — verify with grep first
- Never propose a solution before completing all 5 steps
- If a prior investigation's cited `file:line` references have drifted (file moved, lines shifted), re-investigate that reference — do not guess that it's "probably still right"
- Read `.claude/rules/invariants.md` and `.claude/dev/module-map.md` before starting
