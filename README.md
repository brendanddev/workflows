# WORKFLOWS

A reusable discipline for AI-assisted development.

Hard rules live in files. The model is treated as a stateless emitter.
Agents and skills stay narrow and auditable. Destructive actions are blocked by configuration, not by hoping the model behaves.

**Start with [`WORKFLOW.md`](./WORKFLOW.md)** — it explains the investigation →
implementation → verify → learnings loop this repo packages. Everything else
here (`.claude/`, `docs/`) is the mechanism; that file is the reasoning.

## Using this in a new project
1. Copy `.claude/` and `docs/` into the new project's root.
2. Copy `CLAUDE.md.template` to `CLAUDE.md` and fill it in.
3. Populate `.claude/dev/module-map.md`, `.claude/rules/architecture.md`, and
   `docs/roadmap/README.md` — these ship as empty templates and the rest of
   the discipline leans on them being real.
4. Fill in `.claude/skills/debug-investigation/SKILL.md` with this project's
   actual recurring failure classes as you discover them.

---

