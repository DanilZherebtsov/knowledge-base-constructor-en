# Preset manifest: saas-product

version:       8            # preset manifest version; in the build fingerprint — saas-product@<versions.json>
title-word:    "product"
central-type:  architecture  (added to the base decisions/discovery/synthesis/principles)
authority:     "Code beats the wiki (classes with src/). Citation localization is a base rule (base@26), not restated in S7; its lint check — claim-graph/research only."
work-layers:   [specs/, src/, data/, scripts/]
state-sections:[Stage, Path to goal, In progress now, Next (1–2 weeks), Completed last week, Blockers and risks, Known tech debt]
domain-conv:   "Currency and amount format — from the bootstrap interview (universal question outside any slot, no default); the answer fills S8. No special citation format."
interview:     # INTERVIEW-Q
  - "What is the product and who is it for; what stage is it at?"
  - "What sources will be coming in (interviews/discovery, decision notes, technical documentation)?"
  - "Is there already code (src/) or runtime data (data/)?"
raw-defaults:  [discovery/, decisions/, technical/, (business/), (brainstorms/)]   # WITHOUT `misc/` — that's the base/business catch-all; saas doesn't have one
domain-lint:   "Code drift of the central type: pages with type: architecture that lack the implementation: field or have broken paths in it (code renamed/deleted — the page points into the void)."
close-op:      none   # saas writes no domain close operation to wiki/log.md (explicitly "saas: none" in CLOSE-OP)
mechanics:     [spec-lifecycle, software-engineering]

---

## Slot notes (for the orchestrator)

- **S2 / S3.** Central type — `architecture/`: modules, services, contracts, invariants, multi-step product workflows + the runtime-data contract. Mandatory `## Implementation` section and YAML field `implementation:` (a path check, not a semantic drift detector). Base `discovery/`/`synthesis/`/`principles/` are kept; `decisions/` is universal.
- **S4.** On top of the shared `output/`, the layers of a class with code: `specs/` (task specs, status in frontmatter, flat), `src/` (source of truth about behavior), `data/` (the product's runtime data: prompts, KB, configs, email templates — not in the wiki), `scripts/` (temporary/experimental scripts — **draft code**: neither the product code of `src/` nor the draft documents of `output/`; appears when needed, not created empty at bootstrap).
- **S6 (lifecycle file).** `spec-lifecycle.md` — the task spec flow (backlog in STATE → file in specs/ active → ADR extraction at acceptance → freeze in completed) + sprints (≥3 specs → SPRINT-<NAME>.md) + the symmetric closing of the research layer (the synthesis pass in ingest.md). This is what the `spec-lifecycle` mechanic is.
- **KNOWLEDGE-UNIT.** Default — page (saas does not override to `claim`).
- **ROLES-FILL.** Filled with product-role examples: devops, security-reviewer, support. The roles machinery comes from base (`roles/_template.md`, `roles.md`, and the "Roles" section are always there); no concrete roles are pre-created, but "create role" is available (ADR-0027, revising the deferral from ADR-0004).

## Mechanics (rationale for the pick from {claim-graph, spec-lifecycle, software-engineering, question-lifecycle, decision-lifecycle}; roles are base, not a mechanic)

- **spec-lifecycle** — ON (the unit of work). `methodology/spec-lifecycle.md`, the `specs/` layer, the "Task spec flow" and "Sprints" sections in CLAUDE.md, the S6 pointer.
- **software-engineering** — ON (the code competence). `methodology/software-engineering.md`, the code folder / `src/` / `data/`, the always-on execution-cycle line in the "act" phase, the runtime-`data/` rule, the OWNED-CODE/S4/S7 slots. Works paired with `spec-lifecycle`: the spec is the unit of work, this mechanic is the execution. The `_developer`/`_release-manager` role samples are offered on demand (roles are now always in base — ADR-0027).
- **roles** — **universal base machinery, not a class mechanic** (ADR-0027). `roles/_template.md`, `roles.md`, and the "Roles" section are always present (from base); saas pre-creates no concrete roles, but "create role" works, and the product-role samples from `software-engineering` are offered on demand. The former "roles OFF" (ADR-0004) is lifted.
- **claim-graph** — OFF. The central type is `architecture`, not `claims`; KNOWLEDGE-UNIT = page, not claim.
- **question-lifecycle** — OFF. No `question-lifecycle.md`, CLOSE-OP = none (rather than `question-closed`).
- **decision-lifecycle** — OFF. No `decision-lifecycle.md`, CLOSE-OP = none (rather than `decision-closed`); STATE without a "Commitments calendar".
