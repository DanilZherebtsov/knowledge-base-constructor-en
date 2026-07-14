# Research question flow

A research question is a statement of what needs to be found out: a sub-question of the main research question, a methodological choice that needs working through, or plugging a specific gap in the review.

**Questions have no separate lifecycle folder.** Their state is expressed through STATE.md (the agenda) and the wiki (the result). This is a deliberate refusal of the `output/questions/{open,investigating,answered,abandoned}/` scheme: it would duplicate STATE.md's 7 sections, and parallel lists are forbidden ([state-rules.md](state-rules.md), rule 1). `output/` remains an honest workbench for work-in-progress, not an agenda store.

## Stages and where each one lives

| Stage | Where it lives | What it is |
|---|---|---|
| **Agenda** (stated, not in progress) | STATE.md — "Next" and "Low-priority open questions" | One line. No file. |
| **In progress** | STATE.md "In progress now" + working file `q-NNN-<slug>.md` in `output/` | Draft answer, excerpts, notes along the way. |
| **Answered** | `wiki/claims/` + `wiki/synthesis/`; in STATE — "Answered last week" (buffer) | The canonical result. The working file is disposable. |
| **Abandoned** | removed from STATE; if it's a methodological refusal — an ADR in `wiki/decisions/` | — |

## The active question's working file

When a question is taken into work — a file `output/q-NNN-<slug>.md` is created (numbering is sequential, never reset). It is a working draft, not canonical; it lives "freely" and may be extended, rewritten, split. Structure is free-form, but a useful skeleton:

```markdown
# Q-014 — Full statement of the question

**Why it matters:** which decision or claim depends on it.
**What's needed to answer:** sources, methods, steps.
**Hypothesis:** working version, to be tested along the way.

## [YYYY-MM-DD] working notes
<observations, excerpts, intermediate conclusions>
```

If a question is too large (requires several independent investigations) — split it into sub-questions with a shared prefix (`q-014a-...`, `q-014b-...`); each flows independently.

## The "in progress → answered" transition (the main gesture)

When the answer is formulated and backed by sources, **Claude proposes extracting the canon** (never does it automatically — only upon confirmation):

- Atomic claims into `wiki/claims/` — one answer often yields several, each with its own evidence. Format — [page-conventions.md](page-conventions.md).
- If the answer is substantial (a review, a comparison, a multi-step argument) — `wiki/synthesis/` with the long form, linking to the extracted claims.
- If the answer produced a methodological choice — an ADR in `wiki/decisions/`.
- If the investigation yielded a research rule ("always check a quote against the original", "don't trust sources of type Y") — a principle in `wiki/principles/`.

**This is a mandatory review, not just claim extraction:** go through all four types (`claims/` · `synthesis/` · `decisions/` · `principles/`) and propose what from the answer is worth recording. Skipping it = the wiki falls behind what we have found out (CLAUDE.md "Discipline", item 9).

After extraction, the working file `output/q-NNN-...` becomes disposable: the canon is already in the wiki, the investigation history is in git. In STATE the question moves to "Answered last week", from where a week later it collapses into links to the extracted pages. In `wiki/log.md` — the entry `question-closed | Q-NNN` with links.

## Rules

- **Don't spawn questions speculatively.** A good research question either blocks something or grew out of a specific gap in the review. "It would be interesting to explore X" with no tie to the focus is noise in the agenda. If the focus shifts — we'll state it anew.
- **Reopening.** A source refuting the answer turned up — start a new working file, revisit the linked claims (some may become `superseded`/`invalidated`), update or mark `superseded` the corresponding synthesis.
- **Abandoned question.** If the refusal is a methodological decision ("we decided not to pursue direction X"), record an ADR; otherwise removing it from STATE is enough — the history stays in git.
