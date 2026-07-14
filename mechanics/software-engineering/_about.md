# Mechanic: software-engineering

**What it does (1 line):** gives a class **full competence over its own code** — a home for the code (essentially a root code folder), the "code beats the wiki" rule, the **execution cycle** (task statement → independent review gate by subagents → implementation → repeat gate by running tests → report; loop on errors; blocker escalation), secrets and runtime-data discipline, reproducible deployment, and an on-demand offer of the "edits/deploys" role pair. Writes code **from scratch or on top of existing code**. Does **not** claim the central wiki type; composes with any class lifecycle (paired with `spec-lifecycle` for saas: the spec is the unit of work, this mechanic is the execution).

## Target-project slots it touches

- **OWNED-CODE** (in `methodology/ingest.md`, the gatekeeper's "working code" branch) — *fills.* Replaces the placeholder with a pointer: code-folder layout, conventions, the cycle, and the role offer are governed by `software-engineering.md`.
- **S4** (working layers for classes with code) — *declares* the code folder (named for what the asset is: `landing/`, `src/`, `scripts/`) + lazy `src/`/`data/`/`scripts/`. The `specs/` folder is not from here: `spec-lifecycle` declares it, if the class runs it.
- **S7** (authority rule) — *extends.* Adds to the class authority line: "**code in the code folder beats the wiki** — for the code itself the truth is on disk; only knowledge about it goes into the wiki".
- **always-on cycle** (in `CLAUDE.md`, "How Claude works on tasks → the "act" phase") — *inserts* a pointer line to the execution cycle; otherwise the protocol would be read only by trigger.
- **S2** — *relies* on `decisions/` and `principles/` existing (stack/hosting decisions → ADR; deployment lessons → principles).

## Target-project files it touches

- `methodology/software-engineering.md` — this drop-in is installed (the mechanic's main file).
- `roles/_developer.md`, `roles/_release-manager.md` — samples of the product role pair; `roles/` always exists (base machinery, ADR-0027), the samples are offered via the deployable-web-product gate.
- `methodology/ingest.md` — fill the OWNED-CODE slot (see wiring).
- `CLAUDE.md` — four points: the code-folder line in the "Architecture" tree (S4); the authority-rule extension (S7); the always-on line about the execution cycle (the "act" phase); the `software-engineering.md` pointer in the "Wiki: page types and operations" section. Plus the `data/` rule in "Discipline", if the work layers include `data/`.
- `methodology/bootstrap.md` — the code folder is not created empty; `src/`/`data/` are lazy.
- Creates during operation (not at install): the code folder `<asset>/` (by human decision), a `README.md` inside it, ADRs in `decisions/`, entries in `STATE.md`.

## Mechanics it depends on

- **The base skeleton** — the gatekeeper in `ingest.md` (code recognition + the OWNED-CODE slot), `wiki/{decisions,principles}/`, slots S4/S7, the cycle injection point in the "act" phase. Without them the mechanic has nothing to hook into.
- **roles** — **base machinery, always present** (ADR-0027, not a mechanic dependency). The `_developer`/`_release-manager` samples ship with this mechanic into `roles/`; the "Product roles" section itself offers them via the deployable-web-product gate.
- **The class lifecycle** (S6) — stack/hosting decisions are recorded as ADRs through the class lifecycle (the mechanic does not decide for the owner). For saas it works paired with `spec-lifecycle` (cycle input = the spec); without it, cycle input = a task statement in `STATE.md`/chat.

## Composition

- **With `spec-lifecycle` — composes, does not conflict.** These are two axes: `spec-lifecycle` is the unit of work (spec/sprint, statuses, ADR-on-close, synthesis), `software-engineering` is code competence (the cycle, ownership, deployment). saas has both: the spec gives the what-and-how-we-track, this mechanic — how code gets written and verified. No exclusivity.
- **Does not claim the central wiki type** (unlike spec-/decision-/claim-): an add-on, not a foundation. Compatible with any class lifecycle.

## Step-by-step wiring

1. **Copy the drop-in:** `software-engineering.md` → `<target>/methodology/software-engineering.md`. The samples `roles/_developer.md` and `roles/_release-manager.md` → `<target>/roles/` (the `roles/` folder always exists — base machinery).
2. **Fill the OWNED-CODE slot** in `methodology/ingest.md` (the gatekeeper's "working code" branch). Replace the placeholder `<<SLOT OWNED-CODE: …>>` with: "code-folder layout, code-ownership conventions, the execution cycle, and the role offer are governed by [software-engineering.md](software-engineering.md)".
3. **Declare the code folder (slot S4).** In the "Architecture" tree (`CLAUDE.md`) replace `<<SLOT S4: …>>` (or add next to `output/`) with the line: "`<asset>/` ← The product's production code (named for what it is: `landing/`, `src/`, `scripts/`). Source of truth on disk; maintained and deployed; created by human decision. Details — [methodology/software-engineering.md](methodology/software-engineering.md)." If the class runs `spec-lifecycle`, `specs/` stands next to it (that mechanic declares it). In parallel, in "Documents and naming → Where things go" add: "production code → the root code folder (not `output/`, not `raw/`)".
4. **Extend slot S7** (the authority rule). Add the second half to the class authority line: "**Code in the code folder beats the wiki** — for the code itself the truth is on disk; only knowledge about it goes into the wiki".
5. **Always-on pointer to the cycle.** In "How Claude works on tasks → While working — the "act" phase" add a line (after item 4, "Goal-driven execution"): "**Code — only through the execution cycle**: task statement → independent review by subagents (3 for correctness + 1 for module impact) → implementation → repeat check by running the tests → report; an error → loop back through the cycle; a gap inside scope → into the task statement; a blocker outside scope → to the human with options. In full — [methodology/software-engineering.md](methodology/software-engineering.md)." Without this the protocol is read only "by trigger", and Claude may start coding without opening the mechanic's file.
6. **Pointer in "Wiki: page types and operations" (the always-on entry for both triggers).** After the domain unit-of-work flow add the line: "**Code ownership and the execution cycle** — the code folder (source of truth on disk), secrets discipline, the code-writing cycle, product roles; **working code arrives OR it is decided to build code from scratch → first read [methodology/software-engineering.md](methodology/software-engineering.md)**." This gives the "from scratch" trigger (no ingest) an always-on entry point.
7. **Runtime data (only if the work layers include `data/`).** Insert into the CLAUDE.md "Discipline" section, before the "Knowledge synthesis" rule, the rule: "**Runtime data lives in `data/`, not `wiki/`.** Prompts, KB dictionaries, config files, templates live in top-level `data/`; the runtime consumes them, Claude does not read them as knowledge. Only the **contract** (schema + how it is updated) goes into `wiki/`. Putting a runtime file into `wiki/` is a classic mistake."
8. **Bootstrap.** In `methodology/bootstrap.md`, section "What bootstrap does NOT create", add: "**The code folder** — never created empty; appears when working code arrives or it is decided to build code from scratch, and the human confirms (the gatekeeper in [ingest.md](ingest.md)). `src/`/`data/`/`scripts/` are not created empty either — as needed."
9. **Roles (always present — base).** No edits to `roles.md` required: the `_developer`/`_release-manager` samples are picked up only through the explicit offer in `software-engineering.md` (hard role→sample binding); the default path `roles.md` → `_template.md` for other roles is unchanged.
10. **What NOT to add.** Don't create the code folder empty at bootstrap. Don't duplicate code in `raw/`/`output/`. Don't copy the code itself into `wiki/` (only knowledge about it goes there). Don't duplicate the cycle in the class lifecycle file — it lives here, the lifecycle only links.

## Deactivation (a class without its own code)

1. Don't copy `methodology/software-engineering.md` or the role samples.
2. Leave the OWNED-CODE slot in `ingest.md` in its base variant ("if the project starts seriously owning code — offer to attach the `software-engineering` mechanic").
3. Slots S4/S7 are filled by the preset as usual (S4 empty or for `spec-lifecycle`; S7 — class authority only). Don't insert the cycle line into the "act" phase or the pointer into "Wiki".

## Attach later (at any time)

A project assembled without `software-engineering` can get it at any moment **without reassembly** — the mechanic is additive; nothing in it is exclusive to initialization time.

1. **Trigger:** the human says "we'll be writing code" / "wire up code work"; OR the gatekeeper in `ingest.md` caught incoming code and the human confirmed ownership; OR the bootstrap question about code was answered "yes".
2. **Action:** Claude performs the wiring above (steps 1–9) against the live project — installs `software-engineering.md` (and the product role samples into `roles/` — the folder always exists, base machinery), fills the OWNED-CODE / S4 / S7 slots, adds the always-on cycle line and the pointers, the bootstrap item.
3. **Source of the mechanic's files**, if the constructor scaffolding is already removed: the upstream mirror (the same channel that maintenance/lint uses for SHA version checks). If local scaffolding is in place — take from it.
