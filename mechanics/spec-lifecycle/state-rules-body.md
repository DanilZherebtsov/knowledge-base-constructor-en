# state-rules body for a class with code (spec-lifecycle)

Inserted by wiring into base `state-rules.md`: replaces the slot S5 region ("## Structure") and refines rule 2b. Base rules 1, 3–7 come from base.

## Structure (7 sections)

1. **Stage** — where we are in the lifecycle and what the current goal is (idea / prototype / MVP / beta / v1 / v2).
2. **Path to goal** — milestones from the current moment to the goal, in order. The whole path in one place.
3. **In progress now** — what is in flight right now (usually one or two tasks). The spec file — in `specs/` (`<feature>-NNN-...`, `status: active`). For a sprint — a link to the active sprint spec (`SPRINT-<NAME>.md`) + the active task; **the sprint's task list is not duplicated in STATE** (it lives in the sprint spec).
4. **Next (1–2 weeks)** — the backlog; one-liners here, the spec file is created when the task is taken up.
5. **Completed last week** — the buffer zone before collapsing.
6. **Blockers and risks**.
7. **Known tech debt** — a list of links to ADRs with status `active` and tag `tech-debt`.

**Granularity reflects the horizon of understanding.** At the start, "Path to goal" is 2–3 lines in broad strokes ("formulate the concept", "10 interviews", "build the MVP"). As each item draws closer, it is broken down. Don't atomize prematurely — that is false precision.

## Rule 2b (moving a unit of work)

After a significant item is finished, Claude moves it from "In progress now" to "Completed last week".
