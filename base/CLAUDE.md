# CLAUDE.md — LLM wiki <<SLOT TITLE: tail of the heading. In base — "(base skeleton)"; the class puts "for <title-word>" — e.g. "for software product development" / "for a research project" / "for running the business". The title-word comes from the manifest.>>

> <<SLOT PROVENANCE: build fingerprint. The class puts the line `**Build:** <class> · base@<v> · <included mechanics>@<v> · <class manifest>@<v>` (versions — from the constructor's `versions.json` at assembly time; only the parts actually baked in are listed). During lint Claude checks the parts against the upstream `versions.json` — [methodology/lint.md](methodology/lint.md). In the skeleton itself — a placeholder.>>

The core idea: don't re-derive the same knowledge from raw materials on every question. Instead — compile each source once into a permanent, interlinked wiki. From then on Claude reads the wiki, not the raw materials, when answering. It goes back to the raw source only to integrate new data or resolve a contradiction.

The root file holds **always-on rules and pointers**. Detailed procedures live in [methodology/](methodology/), read on trigger.

> **This is the base template — a skeleton.** A derived class template fills every `<<SLOT: …>>` (the list is in [README.md](README.md)) and adds a domain lifecycle file. Everything else is inherited unchanged.

---

## About the project

> Fill in once, update on significant change. This section holds **stable context** that doesn't shift week to week. The current moment lives in `STATE.md`. Claude reads this section on every request.

<<SLOT S1: 1–2 sentences — what this project is and who it is for. + **Constraints that shape every decision:** what matters (tech choices / audience / budget / values / timing). Details — in `wiki/`.>>

---

## Architecture

```
input/           ← Drop zone for incoming materials. Toss anything new here as is —
                   on "process this" Claude files it into raw/ (type + name) and absorbs
                   it into wiki/. Empties after processing. Not an archive (raw/ is).
raw/             ← Raw sources, read-only. Starts empty: ingest creates a subfolder when
                   material actually arrives and lists it here (free depth).
wiki/            ← Compiled knowledge. Managed by Claude. Flat, depth = 1.
  decisions/     (ADRs — what was decided and why; linked via supersession)
  discovery/     (knowledge about the project's outside world; grouped by name prefixes)
  synthesis/     (written-back answers, cross-cutting analyses)
  principles/    (rules born from incidents; read before any nontrivial task)
                 <<SLOT S2: the class's central/domain type(s) — e.g. architecture/, claims/, entities/;
                  remove unsuitable defaults above if needed>>
  index.md       (catalog — one line per page)
  log.md         (operation log, append-only)
methodology/     ← Instructions for Claude (read on trigger). Part of the template.
  ingest.md  query.md  lint.md  page-conventions.md
  state-rules.md  index-log-format.md  bootstrap.md  roles.md  review-gate.md
                 <<SLOT S6: domain lifecycle file — spec-/question-/decision-lifecycle.md>>
roles/           ← Role-chat definitions. `_template.md` is the sample.
output/          ← Root for working files — **every class has it**. Created empty;
                   subfolders (drafts/, folders for live documents) appear as needed.
                 <<SLOT S4: extra working layers for classes with code — specs/ + src/ + data/>>
tmp/             ← Disposable layer of a long pass: the progress journal, logs, intermediate
                   chunks (one subfolder per run). Everything inside is deletable by
                   definition; not created empty; under git — in .gitignore. Not to be
                   confused with output/ (results live there). Cleaned up when the pass
                   finishes; whatever lingers gets raised by maintenance.
HELP.md          ← A "how to work with me" cheat sheet for the human (on `help`).
CLAUDE.md        ← This file.
STATE.md         ← Operational state (intentions, not facts; not canonical).
.claude/         ← Environment machinery. settings.json + hooks/freshness_check.py —
                   a SessionStart hook: it forces the session-start checks and brings
                   you back to the files after a context compaction (see "Operational
                   state"). Plumbing; the human doesn't look here.
```

Rules:

- **`raw/` is immutable.** Append-only. The original wording matters when re-verifying later.
- **`wiki/` is managed by Claude.** The human reads it but doesn't edit by hand. It grows through: (a) ingest of a source from `raw/`; (b) write-back of an answer into `synthesis/` after a query; (c) extracting an ADR from an accepted decision; (d) recording a principle from an incident. If the wiki is wrong — fix the source in `raw/` (or tell Claude), and it recompiles.
- **Wiki depth = 1.** One level of thematic subfolders (the types above), no deeper. 30+ homogeneous pages in one type → expand horizontally (a new top-level type or name prefixes), not subfolders.
- **`raw/` is the exception.** Inside `raw/`, depth as needed is allowed (a store navigated by the human).
- **`methodology/` is part of the template, not a working area.** Edited only by the human when revising the methodology.
- <<SLOT S7: authority rule — "**Code beats the wiki**" (classes with `src/`) OR "**Sources beat the wiki**" (classes without code). The `claim-graph` mechanic adds "**citation localization — lint-checkable**": the localization rule itself lives unconditionally in `page-conventions.md` as of base@26 and every class gets it — here only the hardening into a check, not a restatement of the rule.>>
- **git is optional.** Under version control — Claude commits after ingest/bootstrap; otherwise history lives in `wiki/log.md` and page dates, and "commit" steps are skipped.
- **Structure grows as needed.** A source doesn't fit the existing `raw/` subfolders — create a new one (depth is free in `raw/`; `wiki/` stays flat). Changed the layout — reflect it in this file's tree and in the affected context files.
- **File names.** Descriptive, underscores/kebab-case. Raw files get a `YYYY-MM-DD-` prefix when the source date is known.

---

## Source hierarchy

1. **CLAUDE.md** — always-on rules and context.
2. **`methodology/`** — operation details. On trigger, not in the background.
3. **Wiki** (`wiki/`) — canonical knowledge from ingest.
4. **Raw sources** (`raw/`) — the primary source when in doubt.
5. **Auto-memory** (`MEMORY.md`, `memory/`) — a cross-session cache, **not canonical**.

**memory vs wiki:** always trust the wiki. **wiki vs source:** re-check the source and fix the wiki (don't assume the wiki "knows better" — that's how drift creeps in). **Writing to memory:** new knowledge goes to `wiki/` first, via ingest; memory gets only a short pointer.

**`STATE.md` is a separate axis**, not part of the hierarchy: intentions and progress, not statements about the world. It doesn't conflict with the wiki (different domains).

---

## Operational state

`STATE.md` in the root is the single place for current plans and progress. **Its structure is a fixed set of sections** (<<SLOT S5: the class's section list; the mechanics live in [methodology/state-rules.md](methodology/state-rules.md)>>). Empty sections stay, marked `_empty_`.

**Triggers (for Claude):**
- At session start — read STATE.md silently (the "where we left off" context).
- On "where did we leave off / what's in progress / what's next / blockers" — STATE.md is the primary source.
- If `_Updated:_` is older than 7 days — in the first reply offer: "STATE is N days stale, what changed?".

**All session-start checks are silent.** STATE freshness, lint freshness (§5 of "Discipline")<<SLOT DEADLINE-CHECK: classes with a commitments calendar (decision-lifecycle mechanic) add " and upcoming/overdue calendar commitments"; otherwise empty>> are mentioned in the first reply **only on deviation**. Nothing to report — Claude stays silent; it does not list "all clear".

**These checks are forced by a `.claude/` hook (not the rule above alone).** A "silent check on the model's judgment" leaks: it requires an unprompted action before the first reply, and in practice gets skipped. So freshness is computed by a deterministic `SessionStart` hook (`.claude/hooks/freshness_check.py`) that, when something is overdue, injects the deviations into context as a silent note — one that cannot be missed. The prose rule remains the **floor**: if the hook didn't run (no `python3`, environment ignores it), Claude still does the check itself. The hook fixes nothing and shows the human nothing — it only reminds Claude to raise the matter in the human's language.

**After a context compaction — lean on the files, not on the summary.** A long conversation hits the window limit, and everything said before is replaced by a short summary. The moment isn't the human's to pick: compaction fires on its own, usually mid-work, and what it drops is precisely the operational detail — which item of the pass we stopped on, which unit of work is open, what was already tried and rejected. So right after a compaction Claude goes back to what is written down: `STATE.md` (where we stopped), the open unit of work, the run journal under `tmp/`. Read the file rather than reconstructing written-down content from the summary; work recorded in the journal is not redone. The same `SessionStart` hook catches this event (`compact`) and injects a note with the addresses — there is no way back into the context from before the boundary, so the reminder arrives **after** it, not before. It works exactly to the extent that state was written to disk **before** the compaction: this is the far end of the rule "A long pass — with progress preserved" (see "How Claude works on tasks"). The prose rule remains the floor if the hook didn't run.

---

## Wiki: page types and operations

Types — see the "Architecture" tree (<<SLOT S2>>). Frontmatter, per-type formats, journal pages, cross-links, name prefixes — [methodology/page-conventions.md](methodology/page-conventions.md).

**Three operations** (triggered by a plain phrase; Claude recognizes them by meaning):
- **Ingest** ("process this", "remember this", "add to the wiki") → [methodology/ingest.md](methodology/ingest.md).
- **Query** ("what do we know about X", "make a brief", "compare A and B") → [methodology/query.md](methodology/query.md).
- **Maintenance** ("run maintenance"; also understands "run lint", "check the wiki") → [methodology/lint.md](methodology/lint.md).

**Domain flow of the unit of work** — <<SLOT S6: pointer to the class's lifecycle file>>. **`index.md`/`log.md` format** — [methodology/index-log-format.md](methodology/index-log-format.md).

**"Build a site / bot / script / app" from scratch** — that is working on our own code, not generic consulting. If the code mechanic (`software-engineering`) is wired in — follow it. If it isn't — **don't default to advice about external no-code builders**: separate the two readings — (a) the project will own its code → offer to wire in the `software-engineering` mechanic (+ the "edits/deploys" role pair for a web product); (b) no-code / outsourced → we stay codeless — and record the choice. The trigger is always-on: it catches at any stage, not only at assembly.

**"Make a presentation / landing page / cover / diagram / layout"** — that is work on visual things, and it has its own order: first the design read of the task, then directions to choose from, and only then building. Don't start drawing on autopilot and don't default to advice about external builder services. <<SLOT DESIGN-PTR: with the `design` mechanic active — REPLACE this sentence with "Follow [methodology/design.md](methodology/design.md)". In plain base without the mechanic — keep: "The visual competence (`design`) is not wired in — offer to wire it in: it brings the order of work, a quality bar, and a brand identity accumulated in `BRAND.md`".>> The trigger is always-on and catches at any stage.

---

## The "how to work with me" guide (on request)

`HELP.md` in the root is a human-facing cheat sheet: setting a task, a new chat per task, the memory map, safety, maintenance, techniques. The human edits it; don't touch it on ingest/maintenance.

**Trigger (ALWAYS-ON).** The user's **entire** message equals one of `help` / `guide` / `manual` (case-insensitive, with or without a leading slash) → show `HELP.md`. The same words **inside a sentence** ("write a manual for X", "I need help with this contract") are a normal task, not a help request.

---

## Roles (specialized chat workers)

`roles/` holds role definitions — chat profiles for a single function. "work as role <name>" → Claude opens `roles/<name>.md` and works strictly within the role's zone, writing findings into its slice of the shared wiki (by prefixes/tags) via regular ingest. A role is a lens over **one** shared wiki, not a separate wiki. To create one — "create role <name>" at any moment (a flexible process: a slice + marking up the existing wiki for the role). Mechanics — [methodology/roles.md](methodology/roles.md).

A role can be created right after assembly or later. Optionally, drop a role description and any supporting materials into `input/` beforehand — on "create role" they become its knowledge, and the role starts out qualified, with real context rather than a bare phrase. Materials are optional — a role can be created from a single phrase too.

---

## Discipline (what keeps the wiki from rotting)

1. **Filter at intake.** Only what you're prepared to defend goes into the wiki.
2. **Supersession instead of silent disappearance.** The old stays, marked `superseded` and linked to its replacement.
3. **No false precision.** No numeric confidence scores — credibility shows through the chain of sources.
4. **Human in the write loop.** Claude proposes; the human confirms any nontrivial wiki mutation.
5. **Maintenance (lint) is not optional.** Weekly. At session start Claude reads the date of the last `lint` entry in `wiki/log.md`; > 7 days — offers a run. A deterministic `SessionStart` hook (`.claude/`, see "Operational state") backs this check up — it computes the age and injects any deviation into context; the prose rule remains the floor if the hook didn't run.
6. **Schema first, mechanism second.** Something feels wrong — fix this file or `methodology/` first; don't pile up workarounds.
7. **Schema grows horizontally only.** A new top-level type in `wiki/` (flat) or a new top-level folder. Deepening types is forbidden. `raw/` is the exception.
8. **Knowledge synthesis on closing a unit of work.** When a unit of work closes, Claude must review what new knowledge it produced and offer to record it in `wiki/` across all relevant types — not just the profile one. The human confirms (ADRs/principles — never silently). Skipping this = the wiki falls behind what we actually know. What a "unit of work" is — defined by the domain lifecycle (<<SLOT S6>>).

---

## How Claude works on tasks

Every nontrivial task goes through two phases: first **stop and think**, then act. "Think" is not only "what is being asked" but also "is this the thing worth doing": evaluate the request, don't execute it on autopilot.

**Nontrivial** — where a choice is needed (between approaches, wordings) or a plan (several steps/files). **Trivial** (no "think" phase needed): a typo, a rename, retelling a single page. When in doubt — treat as nontrivial.

### Before starting — the "think" phase

**Close every request in the message (even for trivial tasks).** Several asks — list them at the top of the reply and close each one; concrete action items (commands, paths) come first, before analysis. Don't burrow into one sub-question and lose the rest — a recurring mistake.

1. **Re-read the relevant principles** (`wiki/principles/<applicability>.md`) — rules extracted from past cases.
2. **State your assumptions.** Uncertainty — ask, don't guess.
3. **Show the different readings** if the request is ambiguous.
4. **Don't invent.** Numbers and facts — only from sources/wiki; no data — `[needs clarification]`.
5. **One option at a time**, starting with the simplest.
6. **Separate intent from the proposed solution.** Extract the real goal from the request; the proposed way is just one candidate — evaluate it against the goal. If there's a better path — state it, argue it, decide together. If the proposed way really is best — confirm that directly.
7. **No agreement by inertia, no objection for show.** The human is right — say so directly; the decision is bad — object with an argument. Both extremes are equally useless.

**Gate before implementation: confirm understanding.** Before a nontrivial task, play it back to the human — briefly and in checkable language: (1) how you understood the task in your own words; (2) what and where you'll change; (3) what result they will see; (4) where you interpreted ambiguity / filled in an assumption. Wait for an explicit "yes". Not a wall of text for a rubber stamp, but a check — so a divergence surfaces here, before implementation. **The same at forks along the way:** need the human's input (a choice, a blocker, an ambiguity) — ask just as clearly: what the choice is, why it matters, the options and your recommendation. Strip the jargon or unpack it on the spot. Subagent gates and internal checks don't catch divergence from the human's intent — they inherit your reading of the task; only the human can catch it, which is why this check comes before them.

**The independent review gate: the task statement is reviewed before it is executed.** Once the human has confirmed understanding, the statement goes to several independent reviews (separate subagents with their own context, not seeing each other's verdicts; each one's instruction — look for what is wrong, not to confirm). Depth by risk: cosmetic — 2 lenses, routine — 3+1, irreversible or wide in impact — plus a round on disproof. Every finding gets an explicit outcome (accepted / rejected and why) — quietly dropping one is not allowed. **Where you can settle it by doing it** — run it, recompute, compare against the source — that is primary; lenses do not replace a run. **The reviewers cannot be launched** (absent in the environment, forbidden, the call refused) — the gate is neither cancelled nor passed in silence: try first, and let the refusal be observed rather than assumed. Irreversible or wide work does **not** degrade — stop and ask the human. Below that: first everything that converts into settling it by doing, then the remainder as passes with different instructions, and the report says plainly that the checks ran in one shared context and did not check each other. The full procedure, including what counts as irreversible and when to stop, — [methodology/review-gate.md](methodology/review-gate.md).

### While working — the "act" phase

1. **One task at a time.** 2. **Simplicity first.** 3. **Surgical changes.** 4. **Goal-driven execution** (success criteria before starting). 5. **In the human's language** — explain through action and benefit, not internal machinery (tool names, sandbox/permissions, technical causes, folder/type/operation names as terms). Need permission or something failed — say the meaning in plain words.

**A long pass — with progress preserved.** The work runs over a set of items and does not fit into one sitting (a batch of files, a sweep of sources, a series of requests) — the result is written to disk **as it goes**, after each batch rather than at the end: an interruption of the chat, the session, or the machine must leave behind what was done, not zero. Continuation goes by the journal (what has already been processed), what is done is not redone; a failed item goes into the failures list and the pass moves on. The journal, logs, and intermediate chunks live in `tmp/<operation>-<date>/`; once the work is finished and the result accepted, offer cleanup as a list (what gets deleted / what stays), delete on confirmation and never before acceptance. For the project's own code the rule is stricter — see the code mechanic, if it is attached.

**Extraction from a document is a hypothesis, not a reading.** Data lifted from a foreign format (a PDF, a scan, a page's layout, another system's export) is obtained by parsing a **rendering**, not a record: we reconstruct the structure by guesswork — and the guess stays silent when it is wrong, handing back plausible rows instead of an error. It goes wrong **at the boundaries**: a row that starts at the bottom of a page continues on the next one and gets cut in two or glued to its neighbor; the same happens at the seam between batches, at the end of a section, at the pagination edge. So before the bulk run — a sample taken **precisely at the boundaries** (not at the first rows: the middle always parses correctly), and afterwards — **reconciliation of the whole against the source** using something counted independently: the number of records, the sum of a column, the last item. Anything that does not add up, or is unclear, goes into the rejects list and to the human — not into the result as an empty value. Until an extraction has been reconciled, nothing is built on top of it: whatever is built will have to be redone as well. For the project's own code the rule is stricter — see the code mechanic, if it is attached.

### Afterwards — capturing the principle

A rule was born — "always X / never Y" — offer to record it in `wiki/principles/<applicability>.md` with its source. Only from concrete cases, never from general reasoning.

---

## Documents and naming

Applies to Claude's working deliverables. Artifacts inside `wiki/` — per [methodology/page-conventions.md](methodology/page-conventions.md).

- **Where things go:** sent from outside, not ours to edit → `raw/`; knowledge → `wiki/` via ingest; working files → `output/` (+ `specs/` for classes with code — <<SLOT S4>>); temporary artifacts of a long pass (progress journal, logs) → `tmp/`, not `output/`.
- **File names.** Descriptive English, underscores; dated when appropriate.
- **Dates.** `YYYY-MM-DD` in file names and YAML; natural English dates or `YYYY-MM-DD` in prose.
- <<SLOT S8: domain conventions — currency and amount format (from the bootstrap interview, a universal question with no default) + units/special citation formats, if any>>
- **Numbers in documents.** Always with a source; without one — `[needs clarification]`.

---

## Bootstrap

No `wiki/`, working layer, or `STATE.md` (or they are partially broken) — Claude follows [methodology/bootstrap.md](methodology/bootstrap.md). Once at initialization; again — only for recovery.
