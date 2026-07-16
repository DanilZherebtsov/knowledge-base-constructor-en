# Mechanic "claim-graph" — atomic statements with an epistemic graph

> Additive mechanic. Base does not have it: base provides generic types (`decisions`/`discovery`/`synthesis`/`principles`) and page-level compilation. claim-graph replaces "unit of knowledge = page" with "unit of knowledge = claim" and adds an explicit graph of epistemic relations between claims.
>
> This is **not a separate target-project file** — the mechanic's pieces are woven into already existing base files (`page-conventions.md`, `ingest.md`, `lint.md`, `CLAUDE.md`). This file gathers them in one place and describes the wiring; what goes where — in [_about.md](_about.md).

The mechanic consists of **four pieces** + the backbone claim-page format.

---

## Backbone format: the `claims/` type

The class's central type. One page = one statement + its evidence chain + links to other claims. This is slot S3 (the central/domain type): described in `page-conventions.md`, declared in the `wiki/` tree of slot S2.

File name — by concept via prefix (grouping replaces subfolders, depth = 1):
- `topic-<topic>.md` — a general statement about a subject
- `hypothesis-<hypothesis>.md` — a testable hypothesis with status `open / validated / invalidated`
- `finding-<finding>.md` — an identified fact, usually from field data
- `definition-<term>.md` — the project's working definition of a term

Body format:

```markdown
# Claim: <one sentence — the statement itself>

**Status:** active | open | validated | invalidated | superseded
**Scope:** where it holds (domain, period, context).
**Does not hold:** where it fails or edge cases (if known).

## Statement

<expanded statement of the claim, 1–3 paragraphs. Precise definitions of terms
are mandatory — either right here or by link to `definition-<term>.md`.>

## Evidence (supporting)

- **(Smith 2024, p. 47)** — brief restatement of what exactly it confirms.
  Link: [Smith 2024 — Architecture of attention](../../raw/literature/2024-smith-architecture-of-attention.md).
- **[interview-jones-2026-04-12.md, min. 14:30]** — respondent Jones confirmed X
  from personal experience. Link: [Jones interview, Apr 2026](../../raw/fieldwork/expert-jones/2026-04-12-interview.md).
- ...

## Counter-evidence (contradicting)

- **(Doe 2023, §4.1)** — Doe gives a counterexample: <short gist>.
  Link: [Doe 2023 — Limits of X](../../raw/literature/2023-doe-limits-of-x.md).
  Claim's response: <how we account for this — narrow the scope, convert to `refines`, or
  downgrade the status>.

## Links

- **Supports:** [claim-attention-bandwidth.md](claim-attention-bandwidth.md) — our claim
  strengthens the more general statement about bandwidth.
- **Contradicts:** [claim-old-model-of-x.md](claim-old-model-of-x.md) — both cannot
  be true at once; see ADR-0008.
- **Refines:** [claim-base-definition-x.md](claim-base-definition-x.md) — we refine
  the broader definition for our scope.

## History

- 2026-04-12 — claim formulated after the Jones interview.
- 2026-05-10 — evidence added from Smith 2024; status moved to `validated`.
- 2026-05-25 — counter-example found in Doe 2023; scope narrowed.
```

**Claim atomicity.** For claims — aim for ≤ 100 lines: a claim that does not fit into one page of that size is most likely composite and should be split into several linked claims via `refines:`. (The general page limits of 400/800 come from base and are not overridden.)

**When to start a new claim rather than extend an existing one.** If a new source says the same thing — add it to the existing claim's Evidence. If it refines within a narrower scope — a new claim with `refines`. If it contradicts — Counter-evidence in the existing claim + potentially a new claim with `contradicts`.

---

## Piece 1 — Frontmatter fields `supports / contradicts / refines`

Added to the base frontmatter **only for `type: claim`**:

```yaml
supports: [claim-attention-bandwidth]      # optional (claim only)
contradicts: [claim-old-model-of-x]        # optional (claim only)
refines: [claim-base-definition-x]         # optional (claim only)
```

This is an explicit graph of epistemic relations between claims:
- `supports` — another claim strengthens this one;
- `contradicts` — another claim contradicts this one;
- `refines` — this claim refines/details another, more general one.

The fields may be empty; early on, a claim often stands in isolation and grows links later.

**Claim statuses** (on top of base `active/superseded`): `open` (statement exists, evidence is weak), `validated` (several independent sources confirm), `invalidated` (a refutation was found; kept as history).

---

## Piece 2 — KNOWLEDGE-UNIT = claim (the compilation unit in ingest)

In `ingest.md`, the slot `<<SLOT KNOWLEDGE-UNIT>>` is filled with the value **`claim`** (instead of the default "page"). This changes two ingest steps:

**The step "extract claim candidates"** (inserted before compilation):
> Go through the source and write out the atomic statements it introduces, confirms, or refutes. For each — state it in one sentence and tie it to a specific location in the source (page / paragraph / timecode). **This is the primary ingest step — claims are the main unit of knowledge in a research project.**

**The step "compile into the wiki"** — a four-branch decision for each claim candidate (replaces base's generic same/new/conflict branches):
- **Same claim as an existing page** → add a new Evidence item to the existing claim. Refresh `sources:` and the `updated:` date.
- **Refines an existing claim within a narrower scope** → create a new claim with `refines: [parent-claim]`. Do not merge into the parent, so the scope does not blur.
- **Contradicts an existing claim** → (a) add Counter-evidence to the existing claim; (b) if the refutation is strong — create a new claim with `contradicts: [old-claim]` and mark the old one `invalidated` or `superseded`; (c) if the situation is ambiguous — surface the disagreement for the human to resolve, do not choose silently.
- **A new concept with no parent in the wiki** → create a new claim. Name it by concept via prefix (`topic-`, `hypothesis-`, `finding-`, `definition-`).

**The cascade** during ingest scans not the generic "same type folder" but specifically `wiki/claims/` and `wiki/synthesis/` for pages whose evidence or contradicts links are materially affected.

**Special case — large field material.** Interview transcripts often contain dozens of potential claims. Do not extract them all in one pass: per one ingest — the 3–5 most important claims; the rest — notes in the raw file itself, a second pass when a relevant query comes back. (This wording is claim-specific; base provides the generic variant about "3–5 key points".)

---

## Piece 3 — DOMAIN-LINT: counter-evidence balance

In `lint.md`, the slot `<<SLOT DOMAIN-LINT>>` (the "Report-only (heuristic)" section) is filled with the central type's check:

> Claims with status `active`/`validated` whose Counter-evidence noticeably outweighs their Evidence — candidates for revision.

Additional claim-specific report checks in the same section:
- Stale claims contradicting a newer source.
- Claims over 100 lines — candidates for splitting via `refines`.

And a claim-specific auto-fix (the "Auto-fix" section):
- Broken claim links (`supports / contradicts / refines`) pointing to a renamed claim with a single candidate → repair.
- Missing reciprocal references: claim A has `supports: [B]`, but B does not mention A → add a mention in the "Links" section of page B.

---

## Piece 4 — citation localization: hardening the base rule into a lint check

The rule "**A citation carries a location**" itself lives in base (`page-conventions.md`) as of base@26 and reaches every class as a write-side rule, with no retroactive check (ADR-0030). Claim-graph does **not restate it — it hardens it**: a claim without localized evidence is unprovable, so here localization is a hard, lint-checkable requirement.

**Hardening (in `page-conventions.md`).** Every item under Evidence / Counter-evidence must contain a localization (page, paragraph, timecode). Without it, the item counts as undocumented and is flagged by lint. Domain localization formats:
- Literature: `(Smith 2024, p. 47)` or `(Smith 2024, §3.2)`
- Field materials: `[interview-jones-2026-04-12.md, min. 14:30]` or `[..., turn 87]`
- Web source: `[Anthropic blog, "Memory" section](https://...)` — with a fragment anchor if possible
- Internal reference: `[claim-attention-bandwidth.md, §evidence-2]`

Without a localization — the marker `[location needed]`.

**Authority rule (S7 in `CLAUDE.md`).** A class without code → "**Sources beat the wiki**" + the claim-graph addition: "**citation localization — lint-checkable**" (the localization rule itself lives in base; here only the hardening into a check).

**Lint check (in `lint.md`, "Report-only" section):**
> **Citations without localization:** a mention of an author/source without a page / paragraph / timecode. Lint outputs the list of such places in the format `<page>:<line number> — <fragment>`.

---

## Wiring (summary)

| Piece | Target file | Slot / place |
|---|---|---|
| Backbone format `claims/` | `page-conventions.md`, `CLAUDE.md` (the `wiki/` tree) | S3 (format), S2 (type declaration) |
| 1. Fields `supports/contradicts/refines` | `page-conventions.md` (frontmatter + "Links") | — (additive to base frontmatter) |
| 2. KNOWLEDGE-UNIT = claim | `ingest.md` | `<<SLOT KNOWLEDGE-UNIT>>` |
| 3. Counter-evidence balance | `lint.md` | `<<SLOT DOMAIN-LINT>>` |
| 4. Citation localization: hardening into lint | `page-conventions.md`, `lint.md`, `CLAUDE.md` | S7 (authority rule), lint report check (the rule itself — in base) |

Step-by-step application order — [_about.md](_about.md).
