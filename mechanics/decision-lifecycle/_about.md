# Mechanic: decision-lifecycle

**What it does.** Defines the class's unit of work = "decision" and its flow (open question in STATE → comparison in `output/drafts/` → ADR in `decisions/` once chosen → cascade into `entities/`/`synthesis/`/the commitments calendar); closing the unit = `decision-closed`.

## Target-project slots it touches

- **S6** (the domain lifecycle file) — **fills it**: places `decision-lifecycle.md` in `methodology/`, makes it the S6 pointer in CLAUDE.md.
- **S2** (the set of `wiki/` type folders) — **requires** the presence of `entities/` (the business domain type on top of the default `decisions`/`discovery`/`synthesis`/`principles`): the decision cascade writes into `entities/`.
- **S5** (STATE sections) — **relies on** the sections "Open tracks", "This week", "Commitments calendar", "Recently completed", "…waiting on me". state-rules declares them; the lifecycle uses them but does not create them.
- CLAUDE.md discipline, rule 8 (synthesis on closing a unit) — **linked**: the "unit of work" of that rule is defined precisely by the S6 file.

## Files it touches

- `methodology/decision-lifecycle.md` — **creates** (this mechanic's file).
- `CLAUDE.md` — the "Wiki: page types and operations" section gets the line "Decision flow … → [methodology/decision-lifecycle.md]"; the S6 pointer "Domain unit-of-work flow" is set to this file.
- The mechanic rewrites no other files: `state-rules.md`, `page-conventions.md`, `index-log-format.md`, `roles.md` only **reference** the lifecycle; their content is not part of this mechanic.

## Which mechanics it depends on

- **roles** (base) — a role prepares a recommendation, but "the choice and the ADR go through decision-lifecycle"; `roles/_template.md` directs writing the accepted decision into `decisions/` via this file. The lifecycle is self-sufficient and works without roles, but roles cannot close decisions without it.
- **STATE mechanics** (base, `state-rules.md`) — the lifecycle keeps its stages in STATE sections and writes recurring commitments into the "Commitments calendar"; the calendar's format and rules are in state-rules.
- **entities as the domain type** (business S2/S3, described in `page-conventions.md`) — the receiver of the decision cascade. Without the `entities/` type, the cascade "chosen counterparty → its page" has nowhere to write.

## Step-by-step wiring (what goes where)

1. **Copy** `decision-lifecycle.md` from this folder into the target project's `methodology/decision-lifecycle.md`.
2. **Fill slot S6** in `CLAUDE.md`:
   - in the "Architecture" tree, in the `methodology/` block, replace `<<SLOT S6: domain lifecycle file …>>` with the line `decision-lifecycle.md (decision flow: question in STATE → comparison → ADR)`;
   - in the "Wiki: page types and operations" section, replace `**Domain unit-of-work flow** — <<SLOT S6: …>>` with `**Decision flow** (open question in STATE → comparison in output/drafts/ → ADR in decisions/ once chosen → cascade into entities/ and principles/) — [methodology/decision-lifecycle.md](methodology/decision-lifecycle.md).`;
   - in "Discipline", rule 8, at the tail `The unit of work is defined by <<SLOT S6>>` — substitute `[methodology/decision-lifecycle.md](methodology/decision-lifecycle.md)`;
   - fill the **DEADLINE-CHECK** slot in the "All session-start checks are silent" section with the text " and upcoming/overdue calendar commitments" (business has a Commitments calendar).
3. **Verify slot S2:** the `wiki/` tree must include the `entities/` type (with grouping by prefixes `supplier-`/`client-`/`contractor-`/`staff-`/`landlord-`/`partner-`). If it is missing — add it (otherwise the cascade has nowhere to write). The entity-card format is in `page-conventions.md` (slot S3).
4. **Verify slot S5:** `state-rules.md` contains the sections "Open tracks", "This week", "Commitments calendar", "Recently completed", and "…waiting on me". The lifecycle relies on them; if the STATE set differs — align the lifecycle's references to the actual section names.
5. **No CLAUDE.md section needs removing** — the mechanic only fills the existing S6 slot; it adds no new always-on sections.
6. **The STATE body (state-rules).** Insert this mechanic's `state-rules-body.md` content into base `state-rules.md`: replace the S5 slot region ("## Structure" with the 7 business sections), refine rule 2b, and **append the section "## Commitments calendar — how it is kept"** (what goes in, line format, replenishment via the cascade, the reminder threshold, behavior on fulfillment). Base rules 1, 3–7 are inherited from base.
7. **Roles (base-level, always present — ADR-0027).** Append to `roles/_template.md` (the "Where to write findings" section) the line "Deadline/commitment → a line in the "Commitments calendar" in `STATE.md`" — it ties roles to the class's signature domain feature.
8. **Check:** `grep -rn "<<SLOT S6" CLAUDE.md` must find nothing; `decision-closed` occurs in `decision-lifecycle.md` and in the log format (`index-log-format.md`, the CLOSE-OP slot).
