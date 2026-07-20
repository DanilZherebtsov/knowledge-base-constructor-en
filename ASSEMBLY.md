# Assembling a project from the constructor

Claude performs this procedure in a **clean room**: it sees ONLY this folder (`base/`, `mechanics/`, `presets/`, the catalog).

## Entry

The normal entry is conversational, via [START-HERE.md](START-HERE.md): an interview that doesn't require naming a class determines the starting preset and extra mechanics. This file (ASSEMBLY) is the mechanics of the assembly itself, which START-HERE relies on (step 4). The direct entry — a preset name (`saas-product` / …) — is the deterministic assembly/test mode.

## Procedure

> **The language of the conversation is sweeping.** Everything the human sees during assembly, incidental progress remarks included, is in their language and free of internal jargon. The rule and its stop-words — [START-HERE.md](START-HERE.md), "The language of the conversation"; step 9 below is one instance of it, not the only place the ban applies (ADR-0030).

1. **Start from base.** Copy `base/` as the project blank (the skeleton with `<<SLOT …>>` markers).
2. **Read the manifest** `presets/<class>.md`.
3. **Substitute the scalar slots** from the manifest into base:
   - `title-word` → the `TITLE` slot in the H1 + the intro prose; `S1` "About the project" — from the interview (step 7).
   - **build fingerprint (provenance)** → the `PROVENANCE` slot in `CLAUDE.md` (line 3). Read the constructor's `versions.json` and write the line from the parts ACTUALLY BAKED IN: `> **Build:** <class> · base@<v> · <each included mechanic>@<v> · <class manifest>@<v>. During lint Claude checks the parts against the upstream `versions.json` — [methodology/lint.md](methodology/lint.md).` Example (saas): `> **Build:** saas-product · base@25 · spec-lifecycle@5 · software-engineering@4 · saas-product@3.` Roles are NOT written into the fingerprint — they are base machinery, not a mechanic (ADR-0027). Optional mechanics not wired in are NOT written. This is the only thing the project needs for updates; the project does not carry the CHANGELOG / full history.
   - `central-type` (S2) → the `wiki/` tree + the type description; `S3` → `page-conventions.md`; `work-layers` (S4) → the tree; `state-sections` (S5) → `STATE.md` + `state-rules.md`; `authority` (S7); `domain-conv` (S8); `INTERVIEW-Q`+`raw-defaults` → `bootstrap.md`; `domain-lint` → `lint.md`; `close-op` → `index-log-format.md`; `KNOWLEDGE-UNIT` → `ingest.md`; `HELP-OPS` (in `HELP.md`) → the list of the parts actually wired in — operations/flows, the "only what's assembled" principle (the same items as the onboarding in step 9, in human language).
   - Slots marked "otherwise empty" that no wired-in mechanic fills (`DEADLINE-CHECK` in a class without `decision-lifecycle`) → remove the marker (resolve to empty).
4. **Bake in the mechanics** from `manifest.mechanics` per their `mechanics/<name>/_about.md` ("Step-by-step wiring" section) + mandatory dependencies (`claim-graph` for `question-lifecycle`).
   - **Resolve a central-type clash yourself, silently** (ADR-0030). Two contenders (`claim-graph` / `spec-lifecycle` / `decision-lifecycle`) — the winner is set by the **criterion** in [mechanics-catalog.md](mechanics-catalog.md) ("Criterion for the central type"): does the project revisit its own conclusions (→ `claim-graph`) or act and move on (→ the class lifecycle). The loser isn't wired in. **Do not put the clash to the human** and do not describe it in internal words — they have no basis for such a choice. What goes out is only the consequence, in their language, and only if there is one for them.
5. **Base machinery is never deactivated.** Roles (`roles.md`, `roles/_template.md`, the "Roles" section, the bootstrap line) are always present — from base (ADR-0027). Fill the `ROLES-FILL` slot with the class's example roles. There are currently no optional mechanics that would require uprooting anything from the base skeleton.
6. **Output contents — what goes into the project.** The assembled project gets ONLY: `CLAUDE.md`, `HELP.md` (the human cheat sheet, with the `HELP-OPS` slot filled), `STATE.md`, `methodology/*.md` (the class set), `roles/` (always — the base role machinery), the working layers per `work-layers` (`specs/` etc., respecting lazy creation — `spec-lifecycle`). **NOT included:** `base/README.md`, `CHANGELOG.md` (single, in the constructor root), and no internal constructor files (`ASSEMBLY.md`, `EXPERIMENT.md`, `mechanics-catalog.md`, `mechanics/`, `presets/`). **The CHANGELOG is an artifact of versioning the constructor repository** (maintained when constructor parts change), NOT a product of per-project assembly. The project carries only the version label line (step 3).
7. **Interview with the user** (bootstrap step 1): instance data — "About the project" (S1), the real sources. Fill in, remove the instructional footnotes and the base insert "This is the base template — a skeleton".
8. **Hygiene.** `grep -rn '<<SLOT' .` and `grep -rn 'ROLES-FILL' .` — empty (the literal `<<SLOT …>>` inside the base prose of `lint.md` is instruction text, not a marker). No broken links to optional mechanics that weren't wired in.
9. **Onboarding (the final handoff).** After assembly and scaffolding cleanup — a short message in the human's language, no internal jargon (never "ingest / query / lint / mechanics / central type / layers / versions"). **Do NOT list the operations line by line** — the detailed list already lives in `HELP.md` (the `HELP-OPS` slot, filled in this assembly; the "only what's assembled" principle is honored there). Give a general orientation in three strokes and send them to the guide for details:
   - "The project is assembled, the constructor's temporary files are cleared away. How to use it:"
   - **Materials** — "New materials — tickets, call notes, emails, changelogs, anything — toss them into the `input/` folder, then say 'process this'. I'll file and absorb them into the project's memory."
   - **A new chat per task** — "Start each new task in a new chat. No need to keep chats — everything important I save into the project's memory on disk (`wiki/`) as we work, and it's visible from any chat."
   - **The guide** — "Anytime, type `guide` — I'll show you in detail what I can do in this project."
   Notes (stage, layers not yet created, git) are NOT put in the handoff — deliberately shorter; the human reads the current state from `STATE.md`, and the full list of capabilities from `HELP.md` via `guide`.

## Result

The project contents (`CLAUDE.md` + `methodology/` + `STATE.md` + working layers), functionally complete per the preset manifest. Byte-for-byte identity is not required — a cosmetic re-telling of base prose is deliberately tolerated; every real customization from the manifest's `customizations_checklist` must be present. `CHANGELOG.md`/`README.md` are outside the assembly (the repo/versioning layer).
