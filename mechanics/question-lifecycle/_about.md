# Mechanic: question-lifecycle (S6 for research)

**What it does (1 line):** defines the research class's unit of work — the research question Q-NNN — and its flow "agenda in STATE → working file in output → canon extraction into wiki" without a separate lifecycle folder.

## Target-project slots it touches

- **S6** (the domain lifecycle file) — filled by this mechanic: `question-lifecycle.md`.
- **S5** (the state-rules body) — the mechanic fills the body of `state-rules.md` with research semantics (see `state-rules-body.md` + wiring step 6): 7 sections, the "Answered" buffer, the rule for moving a question when a claim/synthesis is extracted.
- **S2** (the central/domain wiki type) — requires that the types include `claims/` (plus `synthesis/`, `decisions/`, `principles/`), where the canon is extracted to.

## Files it touches

- `methodology/question-lifecycle.md` — placed (the mechanic itself).
- `methodology/state-rules.md` — body filled in (the S5 region + rule 2b) from `state-rules-body.md`.
- `CLAUDE.md` (target) — the S6 pointer is added and the discipline item "synthesis on closing a unit of work" is verified.
- `STATE.md` (target) — question working files are referenced from sections 3–5 and 7.
- `wiki/claims/`, `wiki/synthesis/`, `wiki/decisions/`, `wiki/principles/`, `wiki/log.md` — the target types the extracted canon lands in, plus the `question-closed | Q-NNN` entry.
- `output/` — the active question's working file `q-NNN-<slug>.md`.

## Which mechanics it depends on

- **claim-graph** (mandatory) — hypothesis statuses (`open`/`validated`/`invalidated`), supersession, claim balance. Installed as a SEPARATE mechanic from `mechanics/claim-graph/`; NOT duplicated here (the contract at the seam: a question is closed by extracting claims, which claim-graph governs).
- **roles** (base-level, always present — ADR-0027) — the question flow is self-sufficient and does not depend on roles: human-in-loop confirmation of nontrivial mutations is set by the base discipline of `CLAUDE.md`. The role machinery is present in every class (research included), but the question flow does not require it.

## Step-by-step wiring

1. **Place the lifecycle file itself.** Copy `question-lifecycle.md` into the target project's `methodology/`. This fills **S6**.
2. **Fill the S6 pointer in CLAUDE.md.** In the "Wiki: page types and operations" block, replace the S6 placeholder with:
   `**Research question flow** (agenda in STATE → working file in output → distillation into wiki) — [methodology/question-lifecycle.md](methodology/question-lifecycle.md).`
   In the "Architecture" tree, replace the S6 line with `question-lifecycle.md (question flow: STATE → output → wiki)`.
3. **Wire up the closing discipline.** Make sure the "Discipline" section of CLAUDE.md has the item "knowledge synthesis on closing a unit of work" and that the "unit of work" is defined as the research question with a link to this file. (In base the item already exists with a link to S6 — no need to add it separately, only verify that S6 points here.)
4. **Ensure the claim-graph dependency (mandatory).** Wire in the claim-graph mechanic (the `wiki/claims/` type + statuses + supersession) per its `_about.md`. Without it the question flow cannot close: canon extraction lands in `claims/`.
5. **roles — base-level, always present (ADR-0027).** No separate step required: `roles.md`/`roles/_template.md`/the "Roles" section come from base for every class. research fills the `ROLES-FILL` slot with research examples (methodologist/fact-checker/reviewer); the question flow does not involve roles.
6. **Fill the STATE body (state-rules).** Insert this mechanic's `state-rules-body.md` content into base `state-rules.md`: replace the S5 slot region ("## Structure" with the 7 research sections + granularity) and refine rule 2b (moving a question "In progress now" → "Answered" when a claim/synthesis is extracted). Base rules 1, 3–7 are inherited from base. Do NOT create a separate `output/questions/{...}/` folder (a deliberate refusal, see the lifecycle).
7. **Remove nothing extra from CLAUDE.md.** The mechanic fills S6 + the state-rules body and relies on the already existing sections "Operational state", "Discipline", "Wiki: types".
