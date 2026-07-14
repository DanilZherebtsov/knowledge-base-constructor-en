# Task spec flow

A spec is the document of a specific feature or task: the problem, what will change, the implementation, the acceptance criteria. Specs live in the top-level `specs/` folder — a **third class of artifacts**: not disposable scratch (like `output/`) and not compiled knowledge (like `wiki/`), but long-lived working documents of development — mutable while the task is active, frozen after.

**Why a separate folder, not `output/` and not status subfolders.** An active spec is a document code is written against for weeks; "a workbench, can be cleaned up" doesn't fit it. Meanwhile `active/completed/rejected/` subfolders would duplicate STATE.md, and parallel lists are forbidden ([state-rules.md](state-rules.md), rule 1). The solution: a flat `specs/`, **status in frontmatter**, STATE.md as the index of active ones.

## Spec frontmatter

```yaml
---
feature: auth
status: active | completed | rejected
updated: YYYY-MM-DD
adr: [adr-0012]          # link to the extracted ADR; appears at completed/rejected
---
```

Status is mandatory — it is how STATE and `grep "status: active" specs/*` find what's in progress.

## Stages

| Stage | status | Where | What |
|---|---|---|---|
| **Backlog** (not taken up) | — | STATE.md "Next" / "Path to goal" | One line. No file. |
| **In progress** | `active` | `specs/<feature>-NNN-<slug>.md` + STATE "In progress now" | Full spec, mutable. |
| **Landed** | `completed` | `specs/...` (frozen) + ADR in `wiki/decisions/` + code in `src/` | Core → ADR; the spec is kept as a snapshot. |
| **Rejected** | `rejected` | `specs/...` (frozen); optionally an ADR | The unique record of "why we did NOT do it". |

## Sprints — bundles of tasks

When a milestone from "Path to goal" requires **≥3 task specs**, it is formalized as a **sprint spec** — a plan container. A single task needs no sprint (needless overhead); a sprint is for work that splits into several verifiable steps.

**File:** `specs/SPRINT-<NAME>.md`, frontmatter `kind: sprint` + `status: pending | active | completed`. Flat in `specs/`, alongside the task specs.

**Contents:** the sprint goal; the **task list** (with status and a link to the task spec once it exists); the sprint's acceptance; a journal of decisions/edits (what we learned along the way). The sprint spec is **mutable while `active`** — it is edited as tasks progress (its nature, unlike a frozen `completed` task spec).

**Execution algorithm:**
1. The sprint spec defines the task set (in broad strokes; detailed as work proceeds). **The plan passes the independent review gate before tasks start** (the same gate as in the execution cycle — [software-engineering.md](software-engineering.md)): is the decomposition right; are the design conditions and assumptions too broad/narrow; which modules the task bundle touches as a whole. Plan-level defects (a wrong gate condition, a missed dependency) are cheapest to catch here — before the first line of code.
2. Take the next task → **write a task spec** (`<feature>-NNN-...`, as usual) → drive it through the **execution cycle** ([software-engineering.md](software-engineering.md): task statement → independent review gate → implementation → repeat gate → report) → close by the task's acceptance.
3. Learned something new along the way → **edit the sprint spec** (add/remove/reword tasks).
4. Next task. When all are closed and the sprint's acceptance is met — the sprint spec → `completed`.

**No duplicates with STATE.** The task list lives **only in the sprint spec**. STATE "In progress now" links to the active sprint + the active task (one line each), without repeating the list; "Path to goal" lists the milestones, the active milestone = the active sprint.

**ADRs** are extracted from individual task specs at their acceptance, as usual. A sprint spec spawns no ADR — it is a plan, not an architectural decision.

**File skeleton** (copy whole; `depends_on`/`blocked_by` are optional):

```
---
kind: sprint
status: pending | active | completed     # pending = created, not yet started
updated: YYYY-MM-DD
depends_on: [SPRINT-X, FEATURE-NNN]      # opt.: sprints/specs this one builds on
blocked_by: "<what blocks the start>"    # opt.: remove once the blocker is cleared
---
# SPRINT-<NAME> — <goal in one line>

**Goal:** <what counts as achieved at the end>.

## Tasks

| #  | Task                                    | Status | Depends |
|----|-----------------------------------------|--------|---------|
| N1 | <broad stroke; task spec when taken up> | 📥     | —       |
| N2 | …                                       | 🟡     | N1      |

Statuses: 📥 queued · 🟡 in progress/partial · ✅ done.
The "Depends" column — task numbers or external specs.

## Sprint acceptance
- [ ] <verifiable exit criterion>
- [ ] Regression gate: <what must not degrade>

## Journal
- **YYYY-MM-DD** — <decision / edit to the task set / finding along the way>.
```

## The working file

When a task is taken up, `specs/<feature>-NNN-<slug>.md` is created (e.g. `auth-012-pkce-flow.md`; numbering is sequential, never reset). While `status: active` it lives freely — extended and rewritten. A large spec can be split into files with a shared prefix (`auth-012-spec.md`, `auth-012-tests.md`, `auth-012-rollout.md`).

## The execution cycle — in the `software-engineering` mechanic

How exactly code gets written against an active spec (task statement → independent review gate by subagents → implementation → repeat gate by running tests → report; loop on errors; escalation of blockers outside scope) is **code competence**, not part of the unit of work. The full protocol — [software-engineering.md](software-engineering.md). The spec serves as the cycle's **input** (its "task statement" step); a sprint plan passes the same gate before tasks start. saas has both mechanics attached, so naming the task is enough ("take task X" / "let's implement X, it's a sprint") — Claude unrolls the cycle on its own. A gap inside scope is written into the active spec (it is mutable); a blocker outside scope is escalated to the human and recorded in the spec's/sprint's `blocked_by:` and in `STATE.md` "Blockers and risks".

## The "active → completed" transition (the main gesture)

**Transition condition.** If the task statement enumerated an exhaustive list of elements ([software-engineering.md](software-engineering.md), "Blocker inside scope"), the transition to `completed` is possible only when **every** element has an outcome — done, or the human explicitly excluded it with the reason recorded in the spec. The `completed` status does not mask silent leftovers.

When the feature has landed in code and the acceptance criteria are met, **Claude offers to extract an ADR** (never automatically — only on confirmation): the architectural core (choice of approach, rejected alternatives, consequences) moves to `wiki/decisions/adr-NNNN-<slug>.md`. Bidirectional links: the ADR's `sources:` points to the spec or PR, the spec's `adr:` field — to the ADR.

Then the spec is **frozen, not deleted**: `status: completed`, and an inoculation against rot is added to its header:

> **⚠ Snapshot of intent at development time (YYYY-MM-DD). Current behavior lives in the code and in [ADR-NNNN](../wiki/decisions/adr-NNNN-....md). This spec is not updated; later changes to the same area get their own specs.**

Why keep it instead of deleting: an ADR is deliberately terse and does not hold the full task statement, the considered edge cases, the rollout plan. The snapshot remains valuable as the history of the iteration. The rot risk (a stale spec looks authoritative) is neutralized by the inoculation — the spec honestly declares itself a snapshot, not a source of truth about current behavior. Future changes to the same code area spawn new specs; the old one remains the record of its iteration.

In STATE the task moves to "Completed last week", from where a week later it collapses into a link to the ADR.

## Assessing new knowledge at completion (mandatory)

**On closing any work — an individual task spec OR a sprint (and also a standalone task without a spec) — Claude must review the new knowledge and propose adding it to `wiki/`.** Not just ADRs. **Walk through all wiki types declared in this project** — the current list is kept by the "Architecture" tree in `CLAUDE.md` (the base set is the five below; the project may have added its own horizontal extensions — walk those too). For each type, ask whether knowledge of that kind was born:

- **decisions/ (ADR)** — was an architectural/product choice made (with rejected alternatives)?
- **architecture/** — did a contract, module, or multi-step product workflow change?
- **discovery/** — did we learn something new about the user, a segment, an input data format, a competitor?
- **synthesis/** — is the outcome of the work worth capturing as a cross-cutting analysis, retrospective, or comparison?
- **principles/** — was a rule born from an incident/near miss ("always X / never Y")?

Claude **proposes** concrete entries (type + title + gist) — the human confirms (CLAUDE.md: "human in the write loop"; ADRs/principles — never silently). The goal is that knowledge from completed work does not stay only in the code/spec/chat but lands in the wiki. Skipping this step = the wiki falls behind what we actually know.

This is the **development layer** of closing. The symmetric closing of the **research layer** (an interview batch, a market/competitor analysis) is the synthesis pass — see [ingest.md](ingest.md).

## Rules

- **Don't spawn specs speculatively.** A full spec is written when the task is taken up or right before that. Ideas for later — one-liners in STATE, not files.
- **`completed`/`rejected` are frozen.** After freezing, substantive changes go through a new ADR or a new spec, not by retroactively editing the frozen file.
- **`rejected` is always recorded when the refusal is meaningful** ("we decided not to do X because Y"). It is the only record of the unbuilt — there will never be code for it, and git shows nothing either. Optionally duplicated by an ADR with `status: rejected` if the decision is large.
- **`specs/` is not `wiki/`.** No mandatory lint/ingest and no `sources:` requirements as in the wiki; these are working documents edited by the human and Claude. But frontmatter with `status` is mandatory.
