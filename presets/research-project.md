# Preset manifest: research-project

version:       7            # CLAUDE.md fingerprint line = "research-project · v7"
title-word:    "research"
central-type:  claims/ (added in place of the base discovery/; the wiki type set = claims/ · decisions/ · synthesis/ · principles/)
authority:     "Sources beat the wiki (S7) + the claim-graph hardening: citation localization — lint-checkable (the localization rule itself — in base@26, do not restate)"
work-layers:   [data/ (opt.), src/ (opt.)]
state-sections:[Stage, Path to goal, In progress now, Next (1–2 weeks), Answered last week, Blockers and risks, Low-priority open questions]
domain-conv:   "Citations with localization: literature `(Smith 2024, p. 47)` / `(Smith 2024, §3.2)`; fieldwork `[interview-jones-2026-04-12.md, min. 14:30]`; web — with an anchor fragment. Without localization — `[location needed]`"
interview:     # INTERVIEW-Q
  - "What is the main research question, and who is the audience of the result?"
  - "What sources will be coming in (literature, field data, correspondence with experts)?"
  - "Will there be your own datasets (data/) or analysis code (src/)?"
raw-defaults:  [literature/, fieldwork/, conversations/, decisions/, datasets/ (opt.)]
domain-lint:   "a claim with status active/validated whose Counter-evidence noticeably outweighs its Evidence (revision candidate) + citations without localization (list of spots `<page>:<line> — <fragment>`)"
close-op:      "question-closed | Q-NNN — with links to the extracted claim/synthesis"
mechanics:     [claim-graph, question-lifecycle, design]

---

## S1 — "About the project" (for assembly)

The context word is "research". Instead of a single "About the project" paragraph — three labeled fields (just as business's S1 carries its own structure):
- **Main research question** — one sentence: what exactly we are finding out.
- **Audience of the result** — who the consumer is; **determines the deliverable genre** (article / report / briefing / dataset).
- **Scope and constraints** — methodological choices, scope, time horizon, data access, ethical boundaries, deadline.

## S3 — format of the central type claims/ (for page-conventions)

One page = one claim + evidence chain + links. Body:

```markdown
# Claim: <one sentence — the claim itself>

**Status:** active | open | validated | invalidated | superseded
**Scope:** where it holds (domain, period, context).
**Does not hold:** boundary cases.

## Statement
## Evidence (supporting)        — each item with citation localization
## Counter-evidence (contradicting) — + "the claim's response"
## Links                        — Supports / Contradicts / Refines
## History                      — dated entries
```

Name prefixes: `topic-`, `hypothesis-`, `finding-`, `definition-`.
Claim statuses: `active / superseded` + the epistemic `open / validated / invalidated`.
Soft claim limit — ≤ 100 lines (composite → split via `refines:`).

## Mechanics actually used by the class

- **claim-graph** — the `supports / contradicts / refines` graph between claims; reverse lookup by `sources:`, the evidence map, the contradiction inventory (special query forms); claim links get repaired at lint.
- **question-lifecycle** — the research question flow: agenda in STATE → working file `output/q-NNN-<slug>.md` → canon extraction into claims/synthesis/decisions/principles. File: `methodology/question-lifecycle.md`.

## Roles and unused mechanics (for assembly control)

- **design** — ON (the visual competence; wired into every preset by default, ADR-0038). `methodology/design.md`, `BRAND.md` at the root (only by consent, offered after acceptance), the OWNED-DESIGN slot in `ingest.md`, the visual branch in the always-on dispatcher, the order line in "the act phase", the action phrase in HELP-OPS. The interview does not ask about it: the question would get a yes almost every time and discriminates nothing. For research that is first of all presentations and diagrams accompanying the results.
- **roles** — **universal base machinery, not a class mechanic** (ADR-0027): `methodology/roles.md`, `roles/_template.md`, the "Roles" section, the bootstrap line — always from base. research **fills `ROLES-FILL`** with research examples (methodologist, fact-checker, skeptical reviewer); no concrete roles are pre-created, "create role" is available. The former refusal (ADR-0004) is lifted by ADR-0027.
- **spec-lifecycle**, **decision-lifecycle** — not used (those belong to saas/business).
