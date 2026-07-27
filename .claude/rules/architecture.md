# Layer Architecture

Fill in this project's actual dependency order and layer rules. Keeping this explicit is what makes `investigation-planner` and `safe-modification.md` checklists concrete instead of guesswork.

## Dependency Order (bottom → top)
`<layer 1> → <layer 2> → <layer 3> → ...`

## Rules
- Lower layers never import from higher layers.
- `<interface/presentation layer>` contains no business logic — dispatch and rendering only.
- State any other layer-ownership rules specific to this project here.

## Known Exceptions
Track deliberate violations of the above as visible tech debt, not silent drift. For each: what breaks the rule, why it was accepted, and whether it's fix-later or permanent.

| Exception | Why | Status |
|-----------|-----|--------|
| `<file:line imports across the boundary>` | `<reason>` | `<tracked tech debt / permanent, justified>` |
