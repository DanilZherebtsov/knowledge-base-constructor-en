# Decision flow

Work is a stream of choices: supplier, location, pricing, hiring, legal structure, stack, hosting, walking away from an option. Every meaningful choice must leave an **ADR** in `wiki/decisions/` — otherwise six months later it's unclear why it was done this way, and the decision quietly gets relitigated.

**The decision is the unit of work for a choice between options** (code we come back to is run by a spec — [spec-lifecycle.md](spec-lifecycle.md)). Closing the unit = `decision-closed`: the choice is made, the canon extracted, the produced knowledge synthesized (see CLAUDE.md "Discipline", the rule on synthesis at closing).

**Decisions have no separate lifecycle folder.** Their state is expressed through `STATE.md` (the agenda) and `wiki/` (the result). This is a deliberate refusal of the `decisions/{open,deciding,done}/` scheme: it would duplicate STATE.md's sections, and parallel lists are forbidden ([state-rules.md](state-rules.md), rule 1).

## Stages and where each one lives

| Stage | Where it lives | What it is |
|---|---|---|
| **Open question** (stated, not in progress) | STATE — "Path to goal" / "Next" / "Blockers, risks, and decisions waiting on me"; the deferred — `BACKLOG.md` | One line. No file. |
| **Under analysis** | STATE "In progress now" + a comparison working file in `output/drafts/` | Gathering options, numbers, pros and cons. Every number with a source. |
| **Decided** | ADR in `wiki/decisions/` + cascade into `entities/`; in STATE → "Recently completed" | The canonical result. |
| **Rejected** (decided NOT to do it) | ADR with `status: rejected` (if the decision is significant) or simply a recorded refusal | The record of what wasn't built — it will exist nowhere else. |

## The comparison working file

When a question is taken up for analysis — a file is created in `output/drafts/`, e.g. `location_comparison_2026-05-25.md`. It is a working draft, not canonical; it lives freely, gets extended and rewritten. `output/drafts/` is a disposable subfolder, not a designated folder: a file unchanged for a month is moved by maintenance to `output/archive/`. While a comparison is being analysed or awaits a decision, its path stands in STATE — that shows the file is alive. A useful skeleton: options as rows, criteria as columns, numbers with sources, a preliminary conclusion.

## The "under analysis → decided" transition (the main gesture)

When the choice is made, **Claude proposes extracting the canon** (never automatically — only upon confirmation):

- **An ADR in `wiki/decisions/`** — the decision itself: context, what was chosen, consequences, rejected alternatives with reasoning. Format and number — [page-conventions.md](page-conventions.md).
- **Cascade into `entities/`** — the chosen counterparty gets or updates its page (terms, a "chosen" mark, a link to the ADR).
- **If the comparison is valuable as a future reference** (suppliers, tariffs) — write it back into `wiki/synthesis/`. Otherwise the file has served its purpose and will in time leave for the archive.
- **Commitment** — if the decision produced a recurring payment or a deadline, add a line to STATE's "Commitments calendar".

In `wiki/log.md` — the entry `decision-closed | <choice>` with a link to the ADR.

## Rules

- **Don't spawn decisions speculatively.** An ADR is written upon an actual choice, not "just in case". Ideas for later — a line in `BACKLOG.md`.
- **Reopening.** New data arrived that changes the choice — start a new working file, a new ADR; the old one is marked `superseded` with a link to the new one (not deleted).
- **A refusal is recorded when it is deliberate** ("we decided not to open a third location, because Y"). It is the only record of what was not done.
