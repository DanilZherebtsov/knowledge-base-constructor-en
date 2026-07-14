# Mechanic: spec-lifecycle

**What it does (1 line):** defines the **unit of work** for projects with code — a spec/sprint in `specs/`, with status in frontmatter, closed by extracting an ADR and synthesizing knowledge across all wiki types. Spec **execution** (the code-writing cycle) is carried by the [`software-engineering`](../software-engineering/_about.md) mechanic: spec-lifecycle covers "what we're doing and how we track it", software-engineering — "how code gets written and verified". saas has both attached.

## Target-project slots it touches

- **S6** (domain lifecycle file) — *fills.* This is exactly the content of slot S6 for classes with code: the `methodology/spec-lifecycle.md` pointer in the "Architecture" tree and in the "Wiki: page types and operations" section.
- **S4** (extra working layers for classes with code: `specs/` + `src/` + `data/`) — *requires* a declared `specs/` folder. This mechanic is a consumer of S4; the folder itself is declared by slot S4 in the tree.
- **S2** (domain wiki type) — *relies* on `architecture/` and `principles/` existing in `wiki/` (the closing synthesis walks all 5 types, including these).

## Target-project files it touches

- `methodology/spec-lifecycle.md` — this drop-in is installed (the mechanic's only new file).
- `CLAUDE.md` — two pointer inserts (see wiring): the "Task spec flow / Sprints" block into the "Wiki: page types and operations" section; the `specs/` rule into "Architecture". (The always-on line about the execution cycle is inserted by the `software-engineering` mechanic, not this one.)
- References (as neighbors in `methodology/`): `state-rules.md`, `ingest.md`. They must be present (inherited from base).
- Creates during operation (not at install): `specs/<feature>-NNN-<slug>.md`, `specs/SPRINT-<NAME>.md`, ADRs in `wiki/decisions/`, entries in `STATE.md`.

## Mechanics it depends on

- **The base skeleton** — `wiki/{decisions,architecture,discovery,synthesis,principles}/`, `methodology/{ingest,state-rules}.md`, the "human in the write loop" discipline. Without them the mechanic dangles.
- **The S4 layer `specs/`** — must be declared in the target project's "Architecture" tree (otherwise specs have nowhere to land).
- No standalone dependencies on `roles` or `claim-graph`.

## Step-by-step wiring

1. **Copy the drop-in:** `spec-lifecycle.md` → `<target>/methodology/spec-lifecycle.md`.
2. **Fill slot S6.** In the "Architecture" tree (`CLAUDE.md`), in the `methodology/` block, replace the placeholder line `<<SLOT S6: domain lifecycle file …>>` with:
   `spec-lifecycle.md    (pending/active/accepted/rejected)`
3. **Fill slot S6 in the "Wiki" section.** Replace `<<SLOT S6: pointer to the class lifecycle file>>` with the flow pointer:
   "**Task spec flow** (backlog in STATE → file in `specs/` with `status: active` → ADR extraction at acceptance, the spec freezes as `completed`) — [methodology/spec-lifecycle.md](methodology/spec-lifecycle.md)."
   Next to it add a **Sprints** paragraph (a milestone of ≥3 task specs → `SPRINT-<NAME>.md`, `kind: sprint`; the task list lives in the sprint spec, not in STATE) with a link to the same file.
4. **Declare `specs/` (slot S4).** Make sure the "Architecture" tree has a top-level `specs/` with the note: flat, one file per task `<feature>-NNN-<slug>.md`, status in frontmatter (NOT in subfolders), STATE.md is the index of active ones. If S4 hasn't been expanded for code yet — expand it (`specs/` + `src/` + `data/`).
5. **Add the rule to "Architecture".** Into the list of rules under the tree insert: "**`specs/` — working task documents, not code and not wiki.** Mutable while `active`; frozen after `completed`/`rejected`. Details — [methodology/spec-lifecycle.md](methodology/spec-lifecycle.md)."
6. **Tie into discipline (the synthesis rule).** The rule "Knowledge synthesis on closing a unit of work" in the "Discipline" section already exists in base; refine its tail: "task spec/sprint → knowledge assessment ([methodology/spec-lifecycle.md](methodology/spec-lifecycle.md))". For classes with code this is the second closing point (the first — research milestone → synthesis pass in `ingest.md`). (The always-on pointer to the execution cycle itself is inserted by the `software-engineering` mechanic — don't duplicate it here.)
7. **Bootstrap/STATE.** Make sure `STATE.md` contains the sections "Path to goal", "In progress now", "Next", "Completed" — the spec relies on them (the backlog and the index of active work). This is slot S5 (`state-rules.md`); no edits to the mechanic itself are needed. **In `bootstrap.md` (slot S4): eager creation at bootstrap — ONLY `specs/.gitkeep`.** Do NOT create `src/`/`data/`/`docs/`/`scripts/` empty; list them in the "What bootstrap does NOT create" section ("appear by human decision, when needed").
8. **STATE body (state-rules).** Insert the contents of this mechanic's `state-rules-body.md` into base `state-rules.md`: replace the slot S5 region ("## Structure" with the 7 dev sections + granularity) and refine rule 2b (moving "In progress now" → "Completed"). Base rules 1, 3–7 are inherited from base.
9. **Runtime data.** The rule "Runtime data lives in `data/`, not `wiki/`" is inserted by the `software-engineering` mechanic (the `data/` layer is part of code ownership). Don't duplicate it here.
10. **Closing a research milestone (the synthesis pass) in `ingest.md`.** saas closes work at two points: task spec/sprint (above) AND research milestone. Append to `ingest.md` the section "## Closing a research milestone: the synthesis pass" — this mechanic's drop-in `ingest-closure-section.md` (consolidating knowledge across sources when a discovery milestone closes; symmetric to closing a spec). This removes the dead links to that section from `spec-lifecycle.md` and Discipline #8.
11. **What NOT to add.** Don't duplicate `specs/active|completed|rejected/` as subfolders — status lives in frontmatter. Don't keep a parallel task list in STATE — it lives in the sprint spec.
