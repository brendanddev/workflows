# Investigations

Investigation reports produced by the `investigation-planner` (or `debug-investigation`) skill get saved here as self-contained files — never left to live only in chat history.

This is a deliberate addition to the discipline this template is based on: in the source project, investigation reports were never persisted, only the eventual session-handoff summary was. That meant a report's full evidence trail vanished once the chat that produced it ended. Saving the report itself fixes that: a later session (or a different model doing implementation) can pick up the exact `file:line` evidence without re-deriving it or trusting a compressed summary.

## Convention
- Filename: `YYYY-MM-DD-short-topic.md`
- One file per investigation, matching the report format from `investigation-planner/SKILL.md` (Reference implementation / Touch points table / Exhaustive matches / Risks / Recommended implementation order)
- If a later implementation pass finds the cited evidence has drifted (files moved, lines shifted), re-investigate and update the report rather than trusting stale line numbers — see `investigation-planner`'s constraints.
- Not every investigation needs a saved file — trivial ones consumed immediately in the same session are fine to leave in chat. Save it when the report will outlive the current conversation: implementation happens in a separate prompt/session, a different model will implement it, or it documents a scope surprise for human review.

See `investigation-template.md` for the exact shape to copy.
