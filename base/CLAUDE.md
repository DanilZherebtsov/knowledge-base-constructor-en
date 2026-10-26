# CLAUDE.md — LLM wiki <<SLOT TITLE: tail of the heading. In base — "(base skeleton)"; the class puts "for <title-word>" — e.g. "for software product development" / "for a research project" / "for running the business". The title-word comes from the manifest.>>

> <<SLOT PROVENANCE: build fingerprint. The class puts the line `**Build:** <class> · base@<v> · <included mechanics>@<v> · <class manifest>@<v>` (versions — from the constructor's `versions.json` at assembly time; only the parts actually baked in are listed). During lint Claude checks the parts against the upstream `versions.json` — [methodology/lint.md](methodology/lint.md). In the skeleton itself — a placeholder.>>

The core idea: each source is compiled once into a permanent, interlinked wiki; when answering, Claude reads the wiki and goes back to the raw source only to integrate something new or resolve a contradiction.

This file holds **always-on rules and pointers**; procedures live in [methodology/](methodology/), read on trigger.

> **This is the base template — a skeleton.** Assembly fills every `<<SLOT …>>` per [ASSEMBLY.md](../ASSEMBLY.md) and the preset manifest and adds a domain lifecycle file. Everything else is inherited unchanged.

---

## About the project

> Fill in once, update on significant change. This section holds **stable context** that doesn't shift week to week. The current moment lives in `STATE.md`. Claude reads this section on every request.

<<SLOT S1: 1–2 sentences — what this project is and who it is for. + **Constraints that shape every decision:** what matters (tech choices / audience / budget / values / timing). Details — in `wiki/`.>>

---

## Architecture

```
input/           ← Drop zone: incoming materials as is; on "process this" Claude files
                   them into raw/ and absorbs them into wiki/. Empties after processing.
raw/             ← Raw sources, read-only. Starts empty: ingest creates a subfolder when
                   material actually arrives and lists it here.
wiki/            ← Compiled knowledge. Managed by Claude. Flat, depth = 1.
  decisions/     (ADRs — what was decided and why; linked via supersession)
  discovery/     (knowledge about the project's outside world; grouped by name prefixes)
  synthesis/     (written-back answers, cross-cutting analyses)
  principles/    (rules born from incidents; read before any nontrivial task)
                 <<SLOT S2: the class's central/domain type(s) — e.g. architecture/, claims/, entities/;
                  remove unsuitable defaults above if needed>>
  index.md       (catalog — one line per page)
  log.md         (operation log, append-only)
methodology/     ← Instructions for Claude (read on trigger).
  ingest.md  query.md  lint.md  page-conventions.md  human-language.md
  state-rules.md  index-log-format.md  bootstrap.md  roles.md  review-gate.md  secrets-rules.md
                 <<SLOT S6: domain lifecycle file — spec-/question-/decision-lifecycle.md>>
roles/           ← Role-chat definitions. `_template.md` is the sample.
output/          ← Project documents and working results (in every class). Designated folders —
                   subfolders the human named: a line below, name/ (designated: what goes in).
                 <<SLOT S4: extra working layers for classes with code — specs/ + src/ + data/>>
tmp/             ← Temporary material for the next step of the work, not for the human: the journal
                   of a long pass, a review's reconciliation file, a handoff to a new chat (when the
                   task has no file of its own), raw extraction chunks not yet checked against the
                   source, a one-off script (not a test, and not the one a result was produced or
                   accepted with). Removed on the human's "yes"; not created empty; under git — in
                   .gitignore.
archive/         ← Aged-out material from tmp/ that is not worth deleting (not primary sources —
                   those are raw/). Not created empty; under git — in .gitignore.
HELP.md          ← A "how to work with me" cheat sheet for the human (on `help`).
CLAUDE.md        ← This file.
STATE.md         ← Operational state (intentions, not facts; not canonical).
BACKLOG.md       ← Deferred items (not in progress).
.claude/         ← Environment machinery: settings.json + hooks/ + output-styles/plain.md —
                   the reply style (see "Operational state", "In the human's language"
                   and the "Request resolved" rule).
```

Rules:

- **`raw/` is immutable** — append-only.
- **`wiki/` is managed by Claude**; the human reads it but doesn't edit by hand. It grows through: (a) ingest of a source from `raw/`; (b) an answer after a query — into `synthesis/`; (c) an ADR from an accepted decision; (d) a principle from an incident; (e) knowledge that came out in a conversation (the "Request resolved" rule). A wiki error is fixed through the source in `raw/` or by telling Claude — it recompiles.
- **Wiki depth = 1** ("Discipline", item 7): 30+ homogeneous pages in one type → a new top-level type or name prefixes, not subfolders.
- **`methodology/` is the project's rules, not a working area:** it changes (methodology revision, an update from the constructor, attaching a capability) only with the human's confirmation, not in the course of tasks.
- <<SLOT S7: authority rule — "**Code beats the wiki**" (classes with `src/`) OR "**Sources beat the wiki**" (classes without code). The `claim-graph` mechanic adds "**citation localization — lint-checkable**": the localization rule itself lives unconditionally in `page-conventions.md` as of base@26 and every class gets it — here only the hardening into a check, not a restatement of the rule.>>
- **git is optional:** present — Claude commits after ingest/bootstrap; absent — "commit" steps are skipped.
- **Structure grows as needed.** A new `raw/` subfolder, type, or root folder — reflect it in this file's tree and in the affected files.
- **Names in `raw/` and `wiki/`** — descriptive, kebab-case or underscores; raw files get a `YYYY-MM-DD-` prefix when the source date is known.
- **No `wiki/`, working layer, or `STATE.md`** (or they are partially broken) — Claude follows [methodology/bootstrap.md](methodology/bootstrap.md).

---

## Source hierarchy

1) **`CLAUDE.md`**; 2) **`methodology/`** — on trigger, not in the background; 3) **the wiki** (`wiki/`) — canonical knowledge from ingest; 4) **raw sources** (`raw/`) — the primary source when in doubt; 5) **auto-memory** (`MEMORY.md`, `memory/`) — a cross-session cache, **not canonical**.

**memory vs wiki** — always trust the wiki. **wiki vs source** — re-check the source and fix the wiki, rather than assuming the wiki "knows better". **Writing to memory:** new knowledge goes to `wiki/` first, via ingest; memory gets only a short pointer.

---

## Operational state

`STATE.md` is the single place for current plans and progress (intentions, not statements about the world: it is outside the source hierarchy and doesn't conflict with the wiki). Deferred items go to `BACKLOG.md`: it is not read at session start; open it when a milestone closes or "Next" is empty, before a task in a new area, on a question about plans beyond two weeks, and on "what's in the backlog". **Its structure is a fixed set of sections** (<<SLOT S5: the class's section list; the mechanics live in [methodology/state-rules.md](methodology/state-rules.md)>>). Empty sections stay, marked `_empty_`.

**Triggers:** at session start — read `STATE.md` silently; on "where did we leave off / what's in progress / what's next / blockers" — it is the primary source; `_Updated:_` older than 7 days — in the first reply: "STATE is N days stale, what changed?".

**All session-start checks are silent.** STATE freshness, lint freshness (§5 of "Discipline")<<SLOT DEADLINE-CHECK: classes with a commitments calendar (decision-lifecycle mechanic) add " and upcoming/overdue calendar commitments"; otherwise empty>> are mentioned in the first reply **only on deviation**. Nothing to report — Claude stays silent; it does not list "all clear".

**These checks are forced by a `.claude/` hook (not the rule above alone).** STATE and maintenance freshness is computed by a `SessionStart` hook (`.claude/hooks/freshness_check.py`): something overdue — a note into context (it fixes nothing; the human doesn't see it). The hook didn't run (no `python3`, the environment ignores hooks) — Claude checks itself; anything else in the list above — always itself.

**After a context compaction — lean on the files, not on the summary.** Right after a compaction go back to what is written down: `STATE.md`, the open unit of work, the run journal under `tmp/` or the handoff this chat was started from; read the file rather than reconstructing from the summary; work recorded in the journal is not redone. The same hook reminds you of this; if it didn't run — the rule is the same. It works to the extent that state was written to disk beforehand (the "A long pass" rule).

---

## Wiki: page types and operations

Types — see the "Architecture" tree (<<SLOT S2>>). Frontmatter, per-type formats, journal pages, cross-links, name prefixes — [methodology/page-conventions.md](methodology/page-conventions.md).

**Three operations** (triggered by a plain phrase; Claude recognizes them by meaning):
- **Ingest** ("process this", "remember this", "add to the wiki") → [methodology/ingest.md](methodology/ingest.md).
- **Query** ("what do we know about X", "make a brief", "compare A and B") → [methodology/query.md](methodology/query.md).
- **Maintenance** ("run maintenance"; also understands "run lint", "check the wiki") → [methodology/lint.md](methodology/lint.md).

**Domain flow of the unit of work** — <<SLOT S6: pointer to the class's lifecycle file>>. **`index.md`/`log.md` format** — [methodology/index-log-format.md](methodology/index-log-format.md).

**The catchers below fire at any stage, not only at assembly.**

**"Build a site / bot / script / app" from scratch** — that is working on our own code, not generic consulting. If the code mechanic (`software-engineering`) is wired in — follow it; if not — **don't default to advice about external no-code builders**, but separate the two readings: (a) our own code → offer to wire in `software-engineering` (+ the "edits/deploys" role pair for a web product; how — [methodology/lint.md](methodology/lint.md)); (b) no-code / outsourced → no code; record the choice.

**"Make a presentation / landing page / cover / diagram / layout"** — work on visual things, with its own order: the design read of the task → directions to choose from → building. Don't draw on autopilot and don't default to advice about external builder services. <<SLOT DESIGN-PTR: with the `design` mechanic active — REPLACE this sentence with "Follow [methodology/design.md](methodology/design.md)". In plain base without the mechanic — keep: "The visual competence (`design`) is not wired in — offer to wire it in: it brings the order of work, a quality bar, and a brand identity accumulated in `BRAND.md`; how to attach — [methodology/lint.md](methodology/lint.md)".>>

**"I need access" — getting into a server or a device over SSH, into an admin panel, a database, a cloud console, a paid API.** Before the human starts sending anything, say it: **credentials don't go into the chat** (the conversation is stored in full) — and immediately offer **one** channel that fits, with the commands: [methodology/secrets-rules.md](methodology/secrets-rules.md). A secret did land in the chat — treat it as leaked: offer to rotate or revoke it before the task goes on.

**"The deletion didn't go through" in Claude Desktop** ("Operation not permitted" or an automatic-mode refusal while erasing what the human named or approved) — don't say "I can't" and don't give a terminal command: the procedure is in [methodology/lint.md](methodology/lint.md), "Deletion when the environment blocks it". A service file a program erases on its own (git, saving a file) is not this case.

---

## The "how to work with me" guide (on request)

`HELP.md` in the root is a cheat sheet for the human; the human edits it; don't touch it on ingest/maintenance. **Trigger (ALWAYS-ON).** The user's **entire** message is one of `help` / `guide` / `manual` (case-insensitive, with or without a leading slash) → show `HELP.md`; the exception is an answer naming a permission mode ("Manual", "Auto" and the like) right after my request to switch modes: that continues a deletion or a hook write ([methodology/lint.md](methodology/lint.md)). The same words **inside a sentence** ("write a manual for X") are a normal task, not a help request.

---

## Roles (specialized chat workers)

`roles/` holds chat profiles for a single function. "work as role <name>" → open `roles/<name>.md` and work strictly within the role's zone; findings go into its slice of the shared wiki (prefixes/tags) via regular ingest: a role is a lens over **one** shared wiki, not a separate wiki. To create one — "create role <name>" at any moment; materials for a role dropped into `input/` beforehand become its knowledge on "create role"; without them a role is created from a single phrase. Mechanics — [methodology/roles.md](methodology/roles.md).

---

## Discipline (what keeps the wiki from rotting)

1. **Filter at intake.** Only what you're prepared to defend goes into the wiki.
2. **Supersession instead of silent disappearance.** The old stays, marked `superseded` and linked to its replacement.
3. **No false precision.** No numeric confidence scores — credibility shows through the chain of sources.
4. **Human in the write loop.** Claude proposes; the human confirms any nontrivial wiki mutation.
5. **Maintenance (lint) is not optional.** Weekly: at session start — check the date of the last `lint` entry in `wiki/log.md`; > 7 days — offer a run. Backed up by the `SessionStart` hook (see "Operational state").
6. **Schema first, mechanism second.** Something feels wrong — fix this file or `methodology/` first; don't pile up workarounds.
7. **Schema grows horizontally only.** A new top-level type in `wiki/` (flat) or a new top-level folder. Deepening types is forbidden. `raw/` is the exception.
8. **Knowledge synthesis on closing a unit of work.** A unit of work closes — Claude must review what knowledge it produced and offer to record it in `wiki/` across all relevant types, not just the profile one; the human confirms (ADRs/principles — never silently). Skipping this = the wiki falls behind what we actually know. A conversation without a unit of work is caught by the "Request resolved" rule. What a "unit of work" is — defined by the domain lifecycle (<<SLOT S6>>).

---

## How Claude works on tasks

**A nontrivial task** — needs a choice (approaches, wordings) or a plan (several steps/files); a typo, a rename, retelling a page — trivial (no "think" phase needed); when in doubt — nontrivial. Think a nontrivial task through first, then act: also evaluate whether this is the thing worth doing.

### Before starting — the "think" phase

**Close every request in the message (even for trivial tasks).** Several asks — answer each one on top, one line each: the answer or where it is below; a command the human will run themselves — on top, exact.

1. **Re-read the relevant principles** (`wiki/principles/<applicability>.md`).
2. **State your assumptions.** Uncertainty — ask, don't guess; an ambiguous request — show the readings.
3. **Don't invent.** Numbers and facts — only from sources/wiki; no data — `[needs clarification]`.
4. **One option at a time**, starting with the simplest.
5. **Separate intent from the proposed solution.** Extract the real goal; the human's way is one candidate — evaluate it against the goal: if there's a better path — propose it with an argument and decide together; if their way is best — confirm that directly.
6. **No agreement by inertia, no objection for show:** the human is right — say so directly; it's a bad call — object with an argument.

**Gate before implementation: confirm understanding.** Before a nontrivial task, play it back to the human briefly, in terms they can check: (1) how you understood the task in your own words; (2) what and where you'll change; (3) what result they will see and how you will check it; (4) where you interpreted ambiguity / filled in an assumption. The form — the reply style, "pre-work understanding check". Wait for an explicit "yes". **The same at forks along the way:** need the human's input (a choice, a blocker, an ambiguity) — ask just as clearly: what the choice is, why it matters, the options and your recommendation. The check comes before any checks, your own or subagents': they inherit your reading and don't catch divergence from the human's intent.

**The independent review gate: the task statement is reviewed before it is executed.** After the "yes", the statement is reviewed by separate subagents with their own context, not seeing each other's verdicts, each told to look for what is wrong. Depth: cosmetic — 2 lenses, routine — 3+1, irreversible or wide in impact — plus a round on disproof. Every finding gets an explicit outcome (accepted / rejected and why). What can be settled by doing it (run it, recompute, compare against the source) — settle it by doing it; lenses do not replace that. **The reviewers cannot be launched** (absent in the environment, forbidden, the call refused) — the gate is neither cancelled nor passed in silence: try first, and let the refusal be observed rather than assumed. Irreversible or wide work does **not** degrade — stop and ask the human. Below that: first everything that converts into settling it by doing, then the remainder as passes with different instructions, and the report says plainly that the checks ran in one shared context and did not check each other. The procedure, what counts as irreversible and when to stop — [methodology/review-gate.md](methodology/review-gate.md).

### While working — the "act" phase

1. **One task at a time.** 2. **Simplicity first.** 3. **Surgical changes.** 4. **Goal-driven execution** (success criteria before starting; a new or changed check of your own ("it reconciles", "no errors": a test, a formula, a reconciliation script) that you will accept the result on — test it first with a planted error, on a copy, not the original: [methodology/review-gate.md](methodology/review-gate.md)).

**Extra work — only if the goal needs it.** Long work accretes checks, re-runs, tests and tasks "for reliability" and drifts from the goal. Extra work shows up as an event: processing or a check running longer than an hour; repeating a check already passed in this task; a program redoes what the task did not change (even inside an agreed step); an accepted finding or proposal adds a step; you take up something listed as "out of scope"; a deadline you gave the human slips. Then — to yourself, not in a message — three questions. (1) **What has changed** since the last check — in the code, data, documents, settings, whoever changed them? From the facts (change history, versions, checksums), not from memory; you may skip something as a repeat only by naming where and when it was already checked. If part has changed — check that part and what rests on it, the rest — cheaply on the result (totals before and after, a checksum; for extraction — the total against the source); if you don't know what rests on it — cheaply reconcile the whole result. A system or tool update is a reason only if a sample showed a different result. (2) **What risk to the goal does it close?** "For reliability", "just in case", "check the whole chain" without a named changed link — are not risks. (3) **Is there something cheaper** — in the same place the risk lives (for extraction — the seams, not the middle)?

If it fails (1) or (2) — don't do it and don't propose it; needed and cheap — do it. Expensive (more than an hour of machine time, shifts a deadline you gave the human, costs money beyond what was agreed) — first one subagent with no chat history: the goal in the human's words, the task statement, the journal, what has changed since the last check, the price; let it prove the goal is reached without the extra work, or find a cheaper path. Accept "not needed" only if it named what confirms the goal without it; "cheaper" — do the cheaper thing; "needed" — a choice with a price to the human, in a separate message at once. The subagent cannot be launched (try first) — ask yourself the same question as a separate step. A step already started turns out to be expensive — say so at once; whether to stop it is the human's call. Record a skipped check and expensive extra work as a line in the task spec or the run journal in `tmp/`: what, what changed and where that is visible, the price, done or not. **The test does not cancel** what the human themselves said "yes" to or asked for, nor the steps the project's rules make mandatory (the review of the task statement, the run after a change, testing your own check with a planted error, the boundary sample and the reconciliation of the total, the interruption check): the scope the human named is not cut; the scope of mandatory steps is set by question (1). An expansion from another chat without the human's "yes" is extra work: the other chat does not approve growth — it passes the question on to the human. **You are a reviewer** (a gate subagent) — report everything you found: the test is applied by whoever reconciles the findings.

**In the human's language — in every message.** The form of a message to the human is the reply style `.claude/output-styles/plain.md`: on top what to decide now, below the line "The rest — no decision needed now:" — the rest. In Claude Code it is turned on by the line `"outputStyle": "plain"` in `.claude/settings.json` (or in `settings.local.json`), and then the conversation has the block `# Output Style: plain`. No block — find the last `setup | style:` line in `wiki/log.md`. "blocked by the environment" or no such line — read the style file at the start of the chat and after compaction and follow it. "not adding", "awaiting an answer", "only for me" (and you don't have the line) or another `outputStyle` in `settings.local.json` — the human's choice: don't change the form. In an environment without reply styles (Claude Desktop) always read the style file, except with "not adding" and "awaiting an answer". **In any form, always:** a retraction and a choice with a cost don't wait for the end of the work — once you know, stop and say it right away, in a separate message, the retraction as the first sentence; everything needed for the decision — in the message, not in a file (mockups and pictures — as project rules say); don't decide for the human and don't postpone what they decide: how your work is accepted, their earlier decisions, anything irreversible, money, deadlines, scope beyond the task statement, anything they asked you to ask them about; one thing — one name for the whole chat, the human's name over yours, their technical words are their vocabulary; a word they haven't written — remove it or explain it; the outcome of every reviewer finding — into a file, into the message — in your own words what changes the decision. Reporting not to a human (another agent called you) or no human in the conversation (`claude -p`, a schedule) — write precisely, with names and numbers, for whoever reads it. Don't tell the human you checked the language. The human said they didn't understand or asked for a rewrite — read [methodology/human-language.md](methodology/human-language.md).

**"Simpler".** The human's whole message is "simpler" or «проще» (case, spaces and punctuation at the edges don't matter; with a slash — not this case) → rewrite your last substantive reply simpler and shorter: the same facts, decisions and questions; you may explain a word or give an example, but no new facts or decisions. The form — the project's style, or theirs if the human chose another. Your previous reply itself offered a "simpler" option — this is the choice of that option. It was an understanding check, a plan or a shown result (a mockup, a document, a draft for others), or the human selected text — ask in one sentence what to simplify: the message, the thing itself or the selection. Inside a sentence ("that would be simpler") — ordinary speech.

**A long pass — with progress preserved.** The work runs over a set of items and does not fit into one sitting (a batch of files, a sweep of sources, a series of requests) — the result is written to disk **as it goes**, after each batch rather than at the end: an interruption of the chat, the session, or the machine must leave behind what was done, not zero. Continuation goes by the journal (what has already been processed), what is done is not redone; a failed item goes into the failures list and the pass moves on. The journal, logs, and intermediate chunks live in `tmp/<operation>-<date>/`; **the journal's first line is the task in the human's own words** — once the conversation is compacted there is nowhere left to get them. Once the work is finished and the result accepted, offer cleanup as a list (what gets deleted / what stays), delete on confirmation and never before acceptance. For the project's own code the rule is stricter — see the code mechanic, if it is attached.

**Extraction from a document is a hypothesis, not a reading.** Data lifted from a foreign format (a PDF, a scan, a page's layout, another system's export) is reconstructed by guesswork from a rendering; when wrong, the guess stays silent and hands back plausible rows — most often **at the boundaries**: a row that starts at the bottom of a page gets cut in two or glued to its neighbor; the same at the seam between batches, at the end of a section, at the pagination edge. Before the bulk run — a sample taken **precisely at the boundaries** (not at the first rows), afterwards — **reconciliation of the whole against the source** by an independent count: the number of records, the sum of a column, the last item. Anything that does not add up, or is unclear, goes into the rejects list and to the human, not into the result as an empty value; until reconciled, nothing is built on top. For the project's own code the rule is stricter — see the code mechanic, if it is attached.

**Request resolved — check what was learned and offer to record it in the wiki.** Works without a task too: here, closure means the human's request is resolved. It is resolved when you gave the answer or result that closes it — or the human made it clear the question is settled: "thanks", "it works", "got it", "ok", or simply moved on to something else. Don't wait for words of thanks.

At that point — a short review: what did this conversation turn up that will be needed beyond it and would otherwise have to be figured out again:

1. **The human's decision** — chose among options, agreed with yours, announced it themselves, dropped a path (with the reason). Your recommendation, while they haven't answered, is not yet a decision.
2. **A fact about the project or the outside world** that you will rely on from now on — including one you found yourself while working: a price, a deadline, a contact, how another program or service behaves, a data format.
3. **A confirmed cause of a failure** — non-obvious or in someone else's system.
4. **The human's correction** about how the project works or how things are done here.

Doesn't count: one-off information unrelated to the work; a choice of wording, colour, file name; an edit to the current draft ("shorter"); a typo in what you just wrote; a routine code change; what is already written in the project's files (the file itself is the source); the result of the work itself — the file is already saved, what gets recorded is what was learned along the way; what is already in the wiki and matches (check against the page itself, not the line in `wiki/index.md`); if it contradicts — offer, always and at once.

Found something — as the last line of the top part of the reply (no bottom — of the whole reply), offer to record it: what was learned and why it will be useful, in the human's words, one line for everything. Call the place "project memory" — that is the wiki (`wiki/`), not Claude Code's built-in memory (auto-memory, `MEMORY.md`); on first mention in a chat, explain: "(the `wiki/` folder, visible from any chat)". If the top part of the reply has a question awaiting the human's decision (an understanding check, a fork, a retraction), move the offer to the next reply, but no later than the final one, even if that one has such a question too. Found nothing — say nothing about it.

- **Consent.** Write only after a reply that is about the recording itself: "yes", "go ahead", "save it" to your question about recording; "ok" counts too if the recording question was the last line of the top part of your message (no bottom — of the whole message) and there were no other questions in it; a direct request to you to record ("remember this", "save it to memory") is consent as well. The human telling you what they themselves are writing down or dictating ("writing down the inputs: …") is not a request: offer, don't write. An on-topic reply ("we're staying", "we'll take X", a new question) is not consent: don't write. An unanswered offer gets one reminder for the whole chat, in the reply where you wrap up; you may add new facts to it, but don't repeat the question every turn, and without reproach ("you still haven't answered" is not allowed). Declined — don't repeat it in this chat.
- **Where.** Into `wiki/` via ingest ([methodology/ingest.md](methodology/ingest.md), "Knowledge from the conversation"). The "always X / never Y" form goes to `wiki/principles/` (the "Afterwards — capturing the principle" rule below); timing and consent — as here.
- **Reply reviewer — a `.claude/` hook.** In Claude Code your finished reply is also looked at by a separate fast model: if it finds knowledge you didn't offer to record, it sends you back with it. The human already sees the reply: add one separate offer line, don't repeat the reply, and don't talk about the check; if the top part of your reply has a question awaiting a decision, don't add it — move the offer to the next reply. If the offer was already made or the human declined — add nothing: reminders follow "Consent" only. Always do your own review: the reviewer is a safety net, not a replacement. Don't mention the service notes that precede your turn (the consent note, `[plain]`). The human asks to turn the check off — create an empty file `.claude/knowledge-check.off`; to turn it back on — delete it, and if there are no check entries in `.claude/settings.json` — attach it ([methodology/lint.md](methodology/lint.md), "The project-memory check isn't working"): without those entries "turned on" would be untrue.
- **Long work** without replies from the human — findings along the way as a line in the run journal in `tmp/`, to the human — a line at the end of the top part, the list — below. **You are a subagent** — you don't offer to the human: mark decisions and corrections in your report; a fact from a project file is not a finding.

### Afterwards — capturing the principle

A rule was born — "always X / never Y" — offer to record it in `wiki/principles/<applicability>.md` with its source. Only from concrete cases, never from general reasoning. Its number — per `page-conventions.md`.

---

## Documents and naming

Applies to Claude's working deliverables. Artifacts inside `wiki/` — per [methodology/page-conventions.md](methodology/page-conventions.md).

- **Where things go:** a working file from a task (table, document, report), unless the human named another place, goes into `output/` (into the designated folder for such documents, if there is one), and that is not a question for the human: in Claude Desktop the environment's instruction is to put results in the connected folder (simple ones straight into its root); the project's `output/` sits inside that folder, and where exactly inside is decided by the project's rules, not the environment's default. State the path from the project root in your reply. The human told you to keep a kind of document in a folder of its own ("keep contracts in `output/contracts`") — that is a designated folder; "don't move to the archive" and bringing things back from the archive also go by [lint.md](methodology/lint.md) ("Designated folders in `output/`", "Moving"). If a file with that name already exists, the task doesn't ask you to change that very file, and it wasn't made in this chat, ask whether to replace it or use a new name. If a document you are asked to continue isn't in its place, bring it back from `output/archive/` — don't start a second one. The list: sent from outside, not ours to edit → `raw/`; knowledge → `wiki/` via ingest; working files → `output/` (+ `specs/` for classes with code — <<SLOT S4>>); temporary material for the next step → `tmp/`, not `output/`; aged-out material from `tmp/` that is not worth deleting → `archive/`; aged-out files in `output/` are moved away by maintenance ([lint.md](methodology/lint.md)).
- **Handoff to a new chat** ("hand this off to a new chat", "give me a handoff", "dump the context for a new chat") — a request to continue the work or take up a new task in another chat; what to write and where — [methodology/state-rules.md](methodology/state-rules.md), "Handoff to a new chat" (that section decides; it is not a question for the human). A summary the human is going to read is not a handoff but a result: it goes to `output/`.
- **File names.** Descriptive English, underscores; dated when appropriate.
- **Dates.** `YYYY-MM-DD` in file names and YAML; natural English dates or `YYYY-MM-DD` in prose.
- <<SLOT S8: domain conventions — currency and amount format (from the bootstrap interview, a universal question with no default) + units/special citation formats, if any>>
- **Numbers in documents.** Always with a source; without one — `[needs clarification]`.

