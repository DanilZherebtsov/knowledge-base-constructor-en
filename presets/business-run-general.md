# Preset manifest: business-run-general

version:       10           # preset manifest version; in the build fingerprint — business-run-general@<versions.json>
title-word:    "the business"
central-type:  entities/  (added to the base decisions/discovery/synthesis/principles)
authority:     "Sources beat the wiki"  (citation localization is a base rule for every class as of base@26; not restated in S7. Its lint check — claim-graph/research only)
work-layers:   []   (output/ only; no code working layer)
state-sections:[Snapshot, Open tracks, This week, Next (1–2 weeks), Commitments calendar, "Blockers, risks, and decisions waiting on me", Recently completed]
domain-conv:   "Currency and amount format — from the bootstrap interview (universal question outside any slot, no default); additionally record the main currency in 'About the project'"
interview:     # INTERVIEW-Q
  - "What is the business, and what is the goal right now?"
  - "Date format and other domain conventions, if any are special."
raw-defaults:  [by the material's concern + misc/]   # business is a class with dynamic raw/: there is no fixed set of names; ingest names the subfolder after what actually arrived
domain-lint:   "Overdue or near-due commitments from STATE.md, 'Commitments calendar' section, that have seen no movement for a long time"
close-op:      "decision-closed | <choice>"
mechanics:     [decision-lifecycle, design]   # roles — universal base machinery, not a class mechanic (ADR-0027)

---

## Slot fill (per the template-anatomy map)

- **S1** — context word "the business"; "About the project" = what the business/venture is + constraints (budget, legal form / tax regime, values/taboos, timing, key people).
- **S2** — the type folder `entities/` (counterparties and people) added to the base four. Grouping by prefixes: `supplier- client- contractor- staff- landlord- partner-`.
- **S3** — the `entities/` format (counterparty card: Status/Who/Contacts/Terms + ## History + ## Links); prefixes for `discovery/`: `market- competitor- norm- price-`. Filled in in page-conventions.md.
- **S4** — empty (working layer = `output/` only, with a `drafts/` subfolder).
- **S5** — 7 fixed STATE sections (see state-sections above) + the expanded "Commitments calendar" machinery (section 5) in state-rules.md.
- **S6** — `decision-lifecycle.md` (question in STATE → comparison in output/drafts/ → ADR in decisions/ → cascade into entities/).
- **S7** — "Sources beat the wiki" (a class without code).
- **S8** — currency and amount format from the bootstrap interview (no default).
- **RAW-DEFAULTS** — dynamic raw/ subfolders (ingest creates them by the concern of the material that arrived, catch-all `misc/`).
- **INTERVIEW-Q** — 2 questions (see interview above).
- **KNOWLEDGE-UNIT** — page (the base default, not `claim`); NOT filled — inherited from base.
- **DOMAIN-LINT** — the check of STATE's "Commitments calendar" (near-due/overdue).
- **CLOSE-OP** — `decision-closed | <choice>` in the log.
- **ROLES-FILL** — role examples: lawyer, finance, procurement, marketer; `roles/_template.md` is completed with the concrete "lawyer" role example.

## Mechanics

- **roles** — **not a class mechanic but universal base machinery** (`methodology/roles.md`, `roles/_template.md`, the "Roles" section in CLAUDE.md — always from base, ADR-0027). The preset only fills `ROLES-FILL` with examples (lawyer/finance/…). The former "business wired roles as an optional mechanic" (ADR-0004/0015) is lifted by ADR-0027 — everyone has roles now.
- **decision-lifecycle** — has `methodology/decision-lifecycle.md` (slot S6), pointers from the CLAUDE.md "Decision flow" section and from "Discipline" item 8.
- **design** — ON (the visual competence; wired into every preset by default, ADR-0038). `methodology/design.md`, `BRAND.md` at the root (only by consent, offered after acceptance), the OWNED-DESIGN slot in `ingest.md`, the visual branch in the always-on dispatcher, the order line in "the act phase", the action phrase in HELP-OPS. The interview does not ask about it: the question would get a yes almost every time and discriminates nothing. For running a business that means presentations above all (to clients, investors, the board) and document styling — a class without code, so the mechanic works standalone, without the code cycle.
- NOT used: claim-graph (research), spec-lifecycle (saas), question-lifecycle (research).
