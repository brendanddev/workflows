---
name: debug-investigation
description: Structured root-cause narrowing for recurring failure classes in this project — matches a symptom to a known entry point, not a fresh grep-everything hunt each time. Fill in the tables below as failure classes are discovered; keep this file current, it is the fastest path to root cause for anyone who hits the same symptom again.
---

## Reference Materials

**Project learnings** (review before diagnosing — known failure patterns and gotchas):
!cat .claude/skills/debug-investigation/learnings.md

---

## When to use this skill
Fill in this project's own recurring symptom categories. Examples of the *shape* (from a prior project — replace with this project's real symptoms):
- Tool/handler fires but produces the wrong result
- A guard/validation rejects something that should pass, or vice versa
- A background/async path silently fails with no visible error
- Config that should change behavior appears to have no effect

## Step 1 — Identify the failure type from available signal
List this project's actual diagnostic signal sources here (log line formats, trace event names, error codes) and what each confirms. Keep it a lookup table, not prose:

| Signal | Confirms |
|--------|----------|
| `<log/trace pattern>` | `<what it tells you>` |

## Step 2 — Match failure to root cause
For each known symptom class, name the exact function/module that owns the decision, and the specific reasons it can produce that symptom. This is the highest-value section — it's what turns "grep everything" into "check these 3 places in order."

**`<symptom>`:**
- Check `<function()>` in `<file>`
- `<condition A>` → `<meaning>`
- `<condition B>` → `<meaning>`

## Step 3 — Key files by failure type

| Failure | Start here |
|---------|-----------|
| `<symptom>` | `path/to/file.ext` — `function_name()` |

## Step 4 — Relevant tests to reference
List existing tests that already exercise each failure class, so a fix can be checked against a known-good regression test instead of writing one from scratch every time.

- `<symptom>`: `test_name()` in `path/to/test_file`

## Step 5 — Common false alarms
Things that look like bugs but are actually expected behavior (cold-start latency, a known intentional no-op, etc.) — save future investigation time by ruling these out first.

- `<symptom that looks like a bug>` — `<why it's actually expected, and how to confirm>`

---

**Do not proceed past root-cause identification into a fix in this same pass unless the fix is trivial and the investigation-planner discipline doesn't apply.** For anything non-trivial, hand root cause + evidence to a separate implementation prompt, per `WORKFLOW.md`.
