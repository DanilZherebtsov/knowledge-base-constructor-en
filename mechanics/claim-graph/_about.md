# Mechanic: claim-graph

**What it does (1 line):** replaces "unit of knowledge = page" with "unit of knowledge = claim" and adds an explicit graph of epistemic links (`supports/contradicts/refines`) between atomic statements, with mandatory localization of every citation.

## Target-project slots it touches

- **S2** — the set of `wiki/` type folders: adds `claims/` (as the central type; for research — instead of `discovery/`).
- **S3** — the central/domain type + its format: claim-evidence chain.
- **S7** — the authority rule: "sources beat the wiki" + the addition "every citation carries a localization, lint-checkable".
- **KNOWLEDGE-UNIT** (in `ingest.md`) — value `claim`.
- **DOMAIN-LINT** (in `lint.md`) — counter-evidence balance for active/validated claims.

## Target-project files it touches

- `methodology/page-conventions.md` — frontmatter (claim fields), the `claims/` type format, the citation rule, localization.
- `methodology/ingest.md` — the KNOWLEDGE-UNIT slot, the "extract claim candidates" step, the 4-branch compilation, the cascade over `claims/`+`synthesis/`, the claim/decision boundary.
- `methodology/lint.md` — the DOMAIN-LINT slot, claim auto-fixes, the report check for citations without localization.
- `methodology/query.md` — special query forms (reverse search by `sources:`, evidence map, contradiction inventory) + distinguishing confidence by claim status/Evidence/Counter-evidence.
- `methodology/index-log-format.md` — the claim index line: one-sentence claim statement + status in parentheses.
- `CLAUDE.md` — the `wiki/` tree (declaring the `claims/` type), the type description in "Wiki: page types", the S7 slot.

## Depends on mechanics

- Nothing hard: claim-graph is additive on top of bare base. Incompatible only with another mechanic claiming the **same** central type (architecture / entities) — claims and architecture cannot coexist as one central type in one project; pick one.

## Step-by-step wiring

1. **Place the mechanic's files.** `mechanics/claim-graph/claim-graph.md` (the whole mechanic) + this `_about.md` are source material for the constructor; they are not copied into the target project as-is — their content is woven in per the steps below.

2. **`CLAUDE.md` → slot S2 (the `wiki/` tree).** In the tree block, add a line for the `claims/` type and (for research) remove `discovery/`:
   ```
   claims/        (atomic statements with an evidence chain; the central type.
                   Grouping via prefixes: topic-, hypothesis-, finding-, definition-)
   ```

3. **`CLAUDE.md` → section "Wiki: page types and operations".** Add a bullet about `claims/` (that it is the central type, one page = a statement + evidence chain + `supports/contradicts/refines` links).

4. **`CLAUDE.md` → slot S7 (authority rule).** Fill with: "**Sources beat the wiki**" + "**every citation carries a localization**, lint-checkable".

5. **`page-conventions.md` → frontmatter.** Add `claim` to the type list; add optional fields `supports / contradicts / refines` marked "claim only"; add `open/validated/invalidated` to the statuses. Insert the paragraph "Links `supports / contradicts / refines`" (Piece 1 of the mechanic).

6. **`page-conventions.md` → slot S3 (type format).** Insert the block "### claims/ — atomic statements" with the name prefixes and the full claim-page body (the mechanic's backbone format), including the "Citation rule" and "When to start a new claim".

7. **`page-conventions.md` → the atomicity section + citation localization.** Add the claim limit of ≤ 100 lines (split via `refines`). The rule "**A citation carries a location**" itself sits unconditionally in base as of base@26 — **do not restate it**; add only the domain localization formats and the hardening: an Evidence/Counter-evidence item without a location counts as undocumented and gets flagged by lint (Piece 4).

8. **`ingest.md` → slot KNOWLEDGE-UNIT.** Fill with the value `claim`. Insert the step "Extract claim candidates" before compilation; replace the generic branches of the "compile" step with 4 branches (same / refines / contradicts / new concept); in "Cascade", specify scanning `wiki/claims/` and `wiki/synthesis/`; add "Special case — field material" (Piece 2). Add the step **"Methodological theses go to `decisions/`"**: if a source carries a methodological choice — it is an ADR, not a claim (boundary: claim = "X is Y"; decision = "we view X through Y"). In the "Place the raw file" step, replace the generic "remove secrets" with research ethics: "Remove respondents' personal data before saving if there is no ethical consent to publish it".

9. **`lint.md` → slot DOMAIN-LINT.** Fill with the check "a claim that is active/validated whose Counter-evidence outweighs its Evidence — a candidate for revision". Under "Auto-fix", add repairing broken claim links and reciprocal references. Under "Report-only", add: claims > 100 lines; stale contradicting claims; **citations without localization** in the format `<page>:<line number> — <fragment>` (Pieces 3 and 4).

10. **`query.md` → special forms.** Add a section "Special query forms": reverse search by `sources:` (what relies on a source), the evidence map around a claim, the contradiction inventory (`contradicts` edges). In the confidence-level step — distinguish by claim status + the Evidence/Counter-evidence balance.

11. **`index-log-format.md` → the claim line.** Clarify: for claims, the summary in the index line = the claim's statement itself in one sentence + status in parentheses, e.g. `- [claim-...] — <statement> _(validated)_`.

12. **What NOT to do.** Do not duplicate base's generic scaffolding into the target project (page atomicity 400/800, cross-links, journal pages, generic ingest steps for raw placement / index / log / commit, generic lint auto/report levels, the update-check gate, the decisions/synthesis/principles/discovery formats) — it is already in base. Only the default `discovery/` is removed from base (its place is taken by `claims/`); the remaining generic types stay.
