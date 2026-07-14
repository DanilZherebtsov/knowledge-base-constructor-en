# Decision flow

A business is a stream of choices: supplier, location, pricing, hiring, legal structure, walking away from an option. Every meaningful choice must leave an **ADR** in `wiki/decisions/` — otherwise six months later it's unclear why it was done this way, and the decision quietly gets relitigated.

**This class's unit of work is the decision.** Closing the unit = `decision-closed`: the choice is made, the canon extracted, the produced knowledge synthesized (see CLAUDE.md "Discipline", the rule on synthesis at closing).

**Decisions have no separate lifecycle folder.** Their state is expressed through `STATE.md` (the agenda) and `wiki/` (the result). This is a deliberate refusal of the `decisions/{open,deciding,done}/` scheme: it would duplicate STATE.md's sections, and parallel lists are forbidden ([state-rules.md](state-rules.md), rule 1).

## Stages and where each one lives

| Stage | Where it lives | What it is |
|---|---|---|
| **Open question** (stated, not in progress) | STATE — "Open tracks" / "Next" / "…waiting on me" | One line. No file. |
| **Under analysis** | STATE "This week" + a comparison working file in `output/drafts/` | Gathering options, numbers, pros and cons. Every number with a source. |
| **Decided** | ADR in `wiki/decisions/` + cascade into `entities/`; in STATE → "Recently completed" | The canonical result. |
| **Rejected** (decided NOT to do it) | ADR with `status: rejected` (if the decision is significant) or simply a recorded refusal | The record of what wasn't built — it will exist nowhere else. |

## The comparison working file

When a question is taken up for analysis — a file is created in `output/drafts/`, e.g. `location_comparison_2026-05-25.md`. It is a working draft, not canonical; it lives freely, gets extended and rewritten. A useful skeleton: options as rows, criteria as columns, numbers with sources, a preliminary conclusion.

## The "under analysis → decided" transition (the main gesture)

When the choice is made, **Claude proposes extracting the canon** (never automatically — only upon confirmation):

- **An ADR in `wiki/decisions/`** — the decision itself: context, what was chosen, consequences, rejected alternatives with reasoning. Format — [page-conventions.md](page-conventions.md).
- **Cascade into `entities/`** — the chosen counterparty gets or updates its page (terms, a "chosen" mark, a link to the ADR).
- **If the comparison is valuable as a future reference** (suppliers, tariffs) — write it back into `wiki/synthesis/`. Otherwise the working file in `output/drafts/` becomes disposable.
- **Commitment** — if the decision produced a recurring payment or a deadline, add a line to STATE's "Commitments calendar".

In `wiki/log.md` — the entry `decision-closed | <topic>` with a link to the ADR.

## Rules

- **Don't spawn decisions speculatively.** An ADR is written upon an actual choice, not "just in case". Ideas for later — one-liners in STATE.
- **Reopening.** New data arrived that changes the choice — start a new working file, a new ADR; the old one is marked `superseded` with a link to the new one (not deleted).
- **A refusal is recorded when it is deliberate** ("we decided not to open a third location, because Y"). It is the only record of what was not done.
