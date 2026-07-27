# Module Map

Fill this in once, early, then keep it current as the codebase grows. It's the authoritative ownership map that `investigation-planner` and `safe-modification.md` point back to — the goal is that "which file owns this decision?" is always a lookup, never a fresh search.

Dependency order (bottom → top): `<fill in — mirrors .claude/rules/architecture.md>`

## `<module or top-level package path>`
Owns: `<what this module is responsible for, in one or two sentences>`
Must not: `<what it must never do — the boundary that keeps this module's scope honest, e.g. "must not parse raw user input" or "must not know about the presentation layer">`
Key files: `<path/to/file.ext>` (`<one-line purpose>`), `<path/to/other_file.ext>` (`<one-line purpose>`)

<!--
Repeat one block per module. Keep each block short — this file is meant to be
read in full before any non-trivial change, so total length matters. If a
module's description grows past a few lines, that's usually a sign it should
be split, or that the detail belongs in a code comment instead.
-->
