# Safe Modification Checklists

Concrete, step-by-step checklists for this project's recurring categories of change. These exist so that "add a new X" or "change how Y works" doesn't require re-deriving the same touch points from scratch every time — fill in the templates below as real checklists emerge, using exact file/function names for this project.

## Template: Adding a New <Capability / Tool / Endpoint / Handler>
1. Add the input/output type(s) in `<file>`.
2. Add the implementation in `<file>`, following the existing pattern in `<reference file>`.
3. Register it wherever dispatch happens: `<file:line>`.
4. Add it to any allow-list, surface, or permission set that gates which capabilities are reachable from which contexts.
5. Add a unit test at the implementation site and an integration test at `<test location>`.

## Template: Changing <a core decision-making pathway>
1. Identify every layer the decision passes through (e.g. classification → acceptance → admission) and change all of them together — changing only one layer is a common source of "half-fixed" bugs.
2. If adding a new case/branch, add it in priority order alongside existing cases, not appended at the end by default.
3. Add an integration test that would have caught the specific regression being fixed.

## Template: Changing <a stateful or mutating pathway>
1. Do not make a mutating action's execution unconditional/immediate if it currently requires approval or a gate — mutations are designed around a propose → approve → execute split; keep that shape.
2. **Revalidate at execution time, not proposal time.** Any approval-gated or previously-validated action must re-check its own preconditions immediately before it executes — not just when it was proposed or queued. State that was valid when a change was proposed (a file's contents, a path's existence, a permission, a resource's availability) can go stale by the time a human approves it or the system gets around to executing it. This is a general security/correctness principle for any system with a time gap between "this was validated" and "this actually runs" — re-check what you're about to do, not what you checked earlier.
3. Preserve rollback semantics for grouped/transactional changes: if part of a batch fails, already-applied parts must roll back, not be left half-applied.
4. After a successful mutation, run this project's verification step and any structural cache/index invalidation the change requires.
5. After a rejected or failed mutation, surface the failure and stop — do not silently re-attempt or partially proceed.
6. Do not weaken an existing allow-list or permission boundary to make a change land more easily — if the boundary is genuinely wrong, that's its own reviewed change, not a side effect of an unrelated one.
