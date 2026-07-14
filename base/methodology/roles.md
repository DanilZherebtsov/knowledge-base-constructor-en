# Roles — specialized chat workers

A role is a chat profile for a single function (examples — <<SLOT ROLES-FILL: the class's roles, e.g.: lawyer, finance, methodologist, security-reviewer>>): "hiring" a function for a task without diluting it with neighboring ones. Definitions live in `roles/<role>.md`; the model file is [roles/_template.md](../roles/_template.md).

## Creating a role — "create role <name>"

**One flexible process, not separate modes.** Whatever the human provides — just a phrase, a role-description file, or that plus a stack of supporting documents — the result is the same: `roles/<role>.md` appears, the existing wiki gets marked up with the role's slice, the provided materials become its knowledge. Only the completeness of the input varies, never the mechanics. Nontrivial mutations — with the human's confirmation.

1. **Assemble the role description** from what was given: a phrase only → synthesize (fill in with 2–4 questions if it is thin); a description file → put it in `raw/`, synthesize from it; both → combine. **Questions to the human — business-level only** (function, boundaries, what materials exist); **the slice (prefixes/tags) and where findings go Claude derives on its own** from the domain and [page-conventions.md](page-conventions.md) — `wiki/` is the agent's internal tool, the human is never asked about tags; the role writes to whichever type fits the finding, not to a single one. Record it in `roles/<role>.md` following [roles/_template.md](../roles/_template.md) (Claude fills in the fields): area of responsibility, boundaries, **slice (prefixes/tags)**, what to read at the start, where to write findings.
2. **Tag/prefix hygiene.** Before fixing the slice — check against what already exists (`grep -rho 'tags:' wiki/`, `ls wiki/<type>/`) and **reuse** rather than breeding synonyms. A new tag/prefix — only when nothing fitting exists.
3. **Knowledge seeding — if supporting documents were given** (a standard, a regulation, the function's norms). They are not preloaded: the human points to them, Claude puts them in `raw/`, studies them in detail and, via a regular [ingest](ingest.md), synthesizes them into the role's home type under the role's prefix/tag.
4. **Init — always, on any input.** A role is often created mid-project, when domain work has already settled into the wiki without its tag. Claude walks the entire wiki once, finds the pages relevant to the role, shows the human a proposal list, and on the confirmed ones sets the tag (renaming to fit the prefix if needed), refreshes `updated:`, appends a line to `wiki/log.md`. **Finish with a short synthesis for the human:** what was assigned to the role and under which tag/prefix (e.g. "assigned 3 pages to the role: …; set the domain tag"); record the key pages found into the role file under "what to read at the start". Without init, a role created late will not see past work in its domain.
5. **Announce the slice** and that the role is invoked with the phrase "work as role <name>".

**No changes to `CLAUDE.md` are needed:** the role mechanism is described there, a new role is picked up from its own file, the role list is `ls roles/`.

## Invocation

"work as role <name>" (no need to name the path — Claude opens `roles/<role>.md` itself), then the task. Claude reads the role file, `CLAUDE.md`, and the wiki starting pages from the role, then works strictly within the role's area (stepping outside it only to redirect).

## A role's knowledge base — a slice of the shared wiki, not a separate wiki

A role has no storage of its own. Its "knowledge base" is a **slice of the one shared wiki** (by prefixes/tags) plus the related raw files. Why not a separate wiki: decisions rely on knowledge from several functions at once — it must live in one graph; a per-function wiki severs links and breeds duplicates.

## Retrieval and saving

- **Retrieval (without reading the whole wiki):** the slice is defined by the role's prefixes/tags. Claude finds pages via `wiki/index.md` and the name prefix (`ls <type>/<prefix>-*`), if needed — `grep` on `tags:`; reads only the matches, one hop along links. New unmarked domain pages get picked up by a repeat init pass, on request.
- **Saving:** new findings — into the shared wiki via a regular [ingest](ingest.md) (with `sources:`, the human confirms); draft analysis — into the class's working layer; a decision — an ADR in `decisions/`.
- **Ongoing learning at any stage:** seeding (step 3 of creation) is not one-off — when the function's standard/practice changes, the human drops in new documents and asks the role to process them; the role ingests them under its tag, replaced versions get marked `superseded` (history is not lost). The role keeps learning on the go, without rewriting the role file.

## Rules

- A role never writes to the wiki silently: via [ingest](ingest.md), with a source.
- A role does not decide for the owner: it prepares a recommendation; the choice and the ADR go through the class's lifecycle file.
- Roles are created and edited by the human (model file: `_template.md`); this is a working area, not methodology.
