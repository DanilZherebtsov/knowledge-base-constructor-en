# CLAUDE.md — LLM wiki <<SLOT TITLE: tail of the heading. In base — "(base skeleton)"; the class puts "for <title-word>" — e.g. "for software product development" / "for a research project" / "for running the business". The title-word comes from the manifest.>>

> <<SLOT PROVENANCE: build fingerprint. The class puts the line `**Build:** <class> · base@<v> · <included mechanics>@<v> · <class manifest>@<v>` (versions — from the constructor's `versions.json` at assembly time; only the parts actually baked in are listed). During lint Claude checks the parts against the upstream `versions.json` — [methodology/lint.md](methodology/lint.md). In the skeleton itself — a placeholder.>>

The core idea: don't re-derive the same knowledge from raw materials on every question. Instead — compile each source once into a permanent, interlinked wiki. From then on Claude reads the wiki, not the raw materials, when answering. It goes back to the raw source only to integrate new data or resolve a contradiction.

The root file holds **always-on rules and pointers**. Detailed procedures live in [methodology/](methodology/), read on trigger.

> **This is the base template — a skeleton.** Assembly fills every `<<SLOT …>>` per [ASSEMBLY.md](../ASSEMBLY.md) and the preset manifest and adds a domain lifecycle file. Everything else is inherited unchanged.

---

## About the project

> Fill in once, update on significant change. This section holds **stable context** that doesn't shift week to week. The current moment lives in `STATE.md`. Claude reads this section on every request.

<<SLOT S1: 1–2 sentences — what this project is and who it is for. + **Constraints that shape every decision:** what matters (tech choices / audience / budget / values / timing). Details — in `wiki/`.>>

---

## Architecture

```
input/           ← Drop zone for incoming materials. Toss anything new here as is —
                   on "process this" Claude files it into raw/ (type + name) and absorbs
                   it into wiki/. Empties after processing; originals then live in raw/.
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
methodology/     ← Instructions for Claude (read on trigger).
  ingest.md  query.md  lint.md  page-conventions.md  human-language.md
  state-rules.md  index-log-format.md  bootstrap.md  roles.md  review-gate.md  secrets-rules.md
                 <<SLOT S6: domain lifecycle file — spec-/question-/decision-lifecycle.md>>
roles/           ← Role-chat definitions. `_template.md` is the sample.
output/          ← Root for working files — **every class has it**. Created empty;
                   subfolders (drafts/, folders for live documents) appear as needed.
                 <<SLOT S4: extra working layers for classes with code — specs/ + src/ + data/>>
tmp/             ← Disposable layer of a long pass: the progress journal, logs, intermediate
                   chunks (one subfolder per run). Everything inside is deletable by definition;
                   not created empty; under git — in .gitignore. Not output/ (results live there).
archive/         ← Aged-out material from output/ and tmp/ that is not worth deleting: things
                   we generated, not primary sources (those are raw/). Not created empty; in .gitignore.
HELP.md          ← A "how to work with me" cheat sheet for the human (on `help`).
CLAUDE.md        ← This file.
STATE.md         ← Operational state (intentions, not facts; not canonical).
.claude/         ← Environment machinery: settings.json + hooks/ (see "Operational state"
                   and the "Request resolved" rule).
```

Rules:

- **`raw/` is immutable.** Append-only. The original wording matters when re-verifying later.
- **`wiki/` is managed by Claude.** The human reads it but doesn't edit by hand. It grows through: (a) ingest of a source from `raw/`; (b) write-back of an answer into `synthesis/` after a query; (c) extracting an ADR from an accepted decision; (d) recording a principle from an incident; (e) knowledge that came out in a conversation (the "Request resolved" rule). If the wiki is wrong — fix the source in `raw/` (or tell Claude), and it recompiles.
- **Wiki depth = 1.** One level of thematic subfolders (the types above), no deeper. 30+ homogeneous pages in one type → expand horizontally (a new top-level type or name prefixes), not subfolders.
- **`methodology/` is the project's rules, not a working area.** It changes when the methodology is revised, on an update from the constructor, or when a capability is attached — with the human's confirmation, not in the course of tasks.
- <<SLOT S7: authority rule — "**Code beats the wiki**" (classes with `src/`) OR "**Sources beat the wiki**" (classes without code). The `claim-graph` mechanic adds "**citation localization — lint-checkable**": the localization rule itself lives unconditionally in `page-conventions.md` as of base@26 and every class gets it — here only the hardening into a check, not a restatement of the rule.>>
- **git is optional.** Under version control — Claude commits after ingest/bootstrap; without it, "commit" steps are skipped.
- **Structure grows as needed.** Created a `raw/` subfolder, a new type, or a root folder — reflect it in this file's tree and in the affected context files.
- **File names in `raw/` and `wiki/`.** Descriptive, underscores/kebab-case. Raw files get a `YYYY-MM-DD-` prefix when the source date is known.

---

## Source hierarchy

1. **CLAUDE.md** — always-on rules and context. 2. **`methodology/`** — operation details, on trigger, not in the background. 3. **Wiki** (`wiki/`) — canonical knowledge from ingest. 4. **Raw sources** (`raw/`) — the primary source when in doubt. 5. **Auto-memory** (`MEMORY.md`, `memory/`) — a cross-session cache, **not canonical**.

**memory vs wiki:** always trust the wiki. **wiki vs source:** re-check the source and fix the wiki (don't assume the wiki "knows better" — that's how drift creeps in). **Writing to memory:** new knowledge goes to `wiki/` first, via ingest; memory gets only a short pointer.

---

## Operational state

`STATE.md` in the root is the single place for current plans and progress: intentions, not statements about the world, so it is outside the source hierarchy and doesn't conflict with the wiki. **Its structure is a fixed set of sections** (<<SLOT S5: the class's section list; the mechanics live in [methodology/state-rules.md](methodology/state-rules.md)>>). Empty sections stay, marked `_empty_`.

**Triggers (for Claude):** at session start — read STATE.md silently (the "where we left off" context); on "where did we leave off / what's in progress / what's next / blockers" — STATE.md is the primary source; if `_Updated:_` is older than 7 days — in the first reply offer: "STATE is N days stale, what changed?".

**All session-start checks are silent.** STATE freshness, lint freshness (§5 of "Discipline")<<SLOT DEADLINE-CHECK: classes with a commitments calendar (decision-lifecycle mechanic) add " and upcoming/overdue calendar commitments"; otherwise empty>> are mentioned in the first reply **only on deviation**. Nothing to report — Claude stays silent; it does not list "all clear".

**These checks are forced by a `.claude/` hook (not the rule above alone).** A check resting on the model's judgment alone gets skipped, so STATE and maintenance freshness is computed by a `SessionStart` hook (`.claude/hooks/freshness_check.py`): something overdue — it injects the deviation into context as a note; it fixes nothing and shows the human nothing. The hook didn't run (no `python3`, the environment ignores hooks) — Claude checks itself; anything else in the list above — always itself.

**After a context compaction — lean on the files, not on the summary.** Compaction fires on its own, mid-work, and drops the operational detail: which item of the pass we stopped on, what is open, what was already tried and rejected. Right after it — go back to what is written down: `STATE.md`, the open unit of work, the run journal under `tmp/`; read the file rather than reconstructing from the summary; work recorded in the journal is not redone. The same hook reminds you of this after a compaction; if it didn't run — the rule is the same. It works to the extent that state was written to disk beforehand (the "A long pass" rule).

---

## Wiki: page types and operations

Types — see the "Architecture" tree (<<SLOT S2>>). Frontmatter, per-type formats, journal pages, cross-links, name prefixes — [methodology/page-conventions.md](methodology/page-conventions.md).

**Three operations** (triggered by a plain phrase; Claude recognizes them by meaning):
- **Ingest** ("process this", "remember this", "add to the wiki") → [methodology/ingest.md](methodology/ingest.md).
- **Query** ("what do we know about X", "make a brief", "compare A and B") → [methodology/query.md](methodology/query.md).
- **Maintenance** ("run maintenance"; also understands "run lint", "check the wiki") → [methodology/lint.md](methodology/lint.md).

**Domain flow of the unit of work** — <<SLOT S6: pointer to the class's lifecycle file>>. **`index.md`/`log.md` format** — [methodology/index-log-format.md](methodology/index-log-format.md).

**The catchers below fire at any stage, not only at assembly.**

**"Build a site / bot / script / app" from scratch** — that is working on our own code, not generic consulting. If the code mechanic (`software-engineering`) is wired in — follow it. If it isn't — **don't default to advice about external no-code builders**: separate the two readings — (a) the project will own its code → offer to wire in the `software-engineering` mechanic (+ the "edits/deploys" role pair for a web product; how to attach — [methodology/lint.md](methodology/lint.md)); (b) no-code / outsourced → we stay codeless — and record the choice.

**"Make a presentation / landing page / cover / diagram / layout"** — that is work on visual things, and it has its own order: first the design read of the task, then directions to choose from, and only then building. Don't start drawing on autopilot and don't default to advice about external builder services. <<SLOT DESIGN-PTR: with the `design` mechanic active — REPLACE this sentence with "Follow [methodology/design.md](methodology/design.md)". In plain base without the mechanic — keep: "The visual competence (`design`) is not wired in — offer to wire it in: it brings the order of work, a quality bar, and a brand identity accumulated in `BRAND.md`; how to attach — [methodology/lint.md](methodology/lint.md)".>>

**"I need access" — getting into a server or a device over SSH, into an admin panel, a database, a cloud console, a paid API.** Before the human starts sending anything, say it: **credentials don't go into the chat** (the conversation is stored in full and cannot be scrubbed after the fact) — and immediately offer **one** channel that fits, with the commands: [methodology/secrets-rules.md](methodology/secrets-rules.md). A secret did land in the chat — treat it as leaked: offer to rotate or revoke it before the task goes on.

**"The deletion didn't go through" in Claude Desktop — "Operation not permitted" or an automatic-mode refusal while erasing files the human named or approved.** Don't say "I can't" and don't give a terminal command: the procedure is in [methodology/lint.md](methodology/lint.md), "Deletion when the environment blocks it". A service file a program erases on its own (git, saving a file) is not this case.

---

## The "how to work with me" guide (on request)

`HELP.md` in the root is a cheat sheet for the human. The human edits it; don't touch it on ingest/maintenance. **Trigger (ALWAYS-ON).** The user's **entire** message equals one of `help` / `guide` / `manual` (case-insensitive, with or without a leading slash) → show `HELP.md`; the exception is an answer naming a permission mode ("Manual", "Auto" and the like) right after my request to switch modes (that continues a deletion or a hook write, [methodology/lint.md](methodology/lint.md)). The same words **inside a sentence** ("write a manual for X", "I need help with this contract") are a normal task, not a help request.

---

## Roles (specialized chat workers)

`roles/` holds role definitions — chat profiles for a single function. "work as role <name>" → Claude opens `roles/<name>.md` and works strictly within the role's zone, writing findings into its slice of the shared wiki (by prefixes/tags) via regular ingest. A role is a lens over **one** shared wiki, not a separate wiki. To create one — "create role <name>" at any moment. Mechanics — [methodology/roles.md](methodology/roles.md).

Materials for a role dropped into `input/` beforehand become its knowledge on "create role"; without them a role is created from a single phrase.

---

## Discipline (what keeps the wiki from rotting)

1. **Filter at intake.** Only what you're prepared to defend goes into the wiki.
2. **Supersession instead of silent disappearance.** The old stays, marked `superseded` and linked to its replacement.
3. **No false precision.** No numeric confidence scores — credibility shows through the chain of sources.
4. **Human in the write loop.** Claude proposes; the human confirms any nontrivial wiki mutation.
5. **Maintenance (lint) is not optional.** Weekly. At session start Claude reads the date of the last `lint` entry in `wiki/log.md`; > 7 days — offers a run. Backed up by the `SessionStart` hook (see "Operational state").
6. **Schema first, mechanism second.** Something feels wrong — fix this file or `methodology/` first; don't pile up workarounds.
7. **Schema grows horizontally only.** A new top-level type in `wiki/` (flat) or a new top-level folder. Deepening types is forbidden. `raw/` is the exception.
8. **Knowledge synthesis on closing a unit of work.** When a unit of work closes, Claude must review what new knowledge it produced and offer to record it in `wiki/` across all relevant types — not just the profile one. The human confirms (ADRs/principles — never silently). Skipping this = the wiki falls behind what we actually know.  A conversation without a unit of work is caught by the "Request resolved" rule ("How Claude works on tasks"). What a "unit of work" is — defined by the domain lifecycle (<<SLOT S6>>).

---

## How Claude works on tasks

Every nontrivial task goes through two phases: first **stop and think**, then act. "Think" is not only "what is being asked" but also "is this the thing worth doing": evaluate the request, don't execute it on autopilot. **Nontrivial** — where a choice is needed (between approaches, wordings) or a plan (several steps/files). **Trivial** (no "think" phase needed): a typo, a rename, retelling a single page. When in doubt — treat as nontrivial.

### Before starting — the "think" phase

**Close every request in the message (even for trivial tasks).** Several asks — list them at the top of the reply and close each one; concrete action items (commands, paths) come first, before analysis. Don't burrow into one sub-question and lose the rest — a recurring mistake.

1. **Re-read the relevant principles** (`wiki/principles/<applicability>.md`) — rules extracted from past cases.
2. **State your assumptions.** Uncertainty — ask, don't guess.
3. **Show the different readings** if the request is ambiguous.
4. **Don't invent.** Numbers and facts — only from sources/wiki; no data — `[needs clarification]`.
5. **One option at a time**, starting with the simplest.
6. **Separate intent from the proposed solution.** Extract the real goal from the request; the proposed way is just one candidate — evaluate it against the goal. If there's a better path — state it, argue it, decide together. If the proposed way really is best — confirm that directly.
7. **No agreement by inertia, no objection for show.** The human is right — say so directly; the decision is bad — object with an argument. Both extremes are equally useless.

**Gate before implementation: confirm understanding.** Before a nontrivial task, play it back to the human — briefly and in checkable language: (1) how you understood the task in your own words; (2) what and where you'll change; (3) what result they will see; (4) where you interpreted ambiguity / filled in an assumption. Wait for an explicit "yes". Not a wall of text for a rubber stamp, but a check — so a divergence surfaces here, before implementation. **The same at forks along the way:** need the human's input (a choice, a blocker, an ambiguity) — ask just as clearly: what the choice is, why it matters, the options and your recommendation. Subagent gates and internal checks don't catch divergence from the human's intent — they inherit your reading of the task; only the human can catch it, which is why this check comes before them.

**The independent review gate: the task statement is reviewed before it is executed.** Once the human has confirmed understanding, the statement goes to several independent reviews (separate subagents with their own context, not seeing each other's verdicts; each one's instruction — look for what is wrong, not to confirm). Depth by risk: cosmetic — 2 lenses, routine — 3+1, irreversible or wide in impact — plus a round on disproof. Every finding gets an explicit outcome (accepted / rejected and why) — quietly dropping one is not allowed. **Where you can settle it by doing it** — run it, recompute, compare against the source — that is primary; lenses do not replace a run. **The reviewers cannot be launched** (absent in the environment, forbidden, the call refused) — the gate is neither cancelled nor passed in silence: try first, and let the refusal be observed rather than assumed. Irreversible or wide work does **not** degrade — stop and ask the human. Below that: first everything that converts into settling it by doing, then the remainder as passes with different instructions, and the report says plainly that the checks ran in one shared context and did not check each other. The full procedure, including what counts as irreversible and when to stop, — [methodology/review-gate.md](methodology/review-gate.md).

### While working — the "act" phase

1. **One task at a time.** 2. **Simplicity first.** 3. **Surgical changes.** 4. **Goal-driven execution** (success criteria before starting).

**Extra work — only if the goal needs it.** The longer the work runs, the easier it is to add one more check, re-run, test, or task "for reliability" and drift away from what the work was started for. Extra work shows up as an event: you launch processing or a check that runs longer than an hour; you repeat a check already passed in this task; a program redoes what the task did not change (that is extra work even inside an agreed step); you accept a finding or proposal that adds a step; you take up something listed as "out of scope"; a deadline you gave the human slips. Then — to yourself, not in a message — three questions. (1) **What has changed** since the last check — in the code, data, documents, settings, whoever changed them? Establish it from the facts (change history, versions, checksums), not from memory; you may skip something as a repeat only by naming where and when it was already checked. If part has changed — check that part and what rests on it, and confirm the rest cheaply on the result (totals before and after, a checksum; for extraction — the total against the source): 2% of a 100-million-record database changed — look at those 2%. If you don't know what rests on the changed part — cheaply reconcile the whole result. A system or tool update is a reason only if a sample showed a different result. (2) **What risk to the goal does it close?** "For reliability", "just in case", "check the whole chain" without a named changed link — are not risks. (3) **Is there something cheaper** — that hits the same place the risk lives (for extraction — the seams, not the middle)?

If it fails (1) or (2) — don't do it and don't propose it. Needed and cheap — do it. Expensive (more than an hour of machine time, shifts a deadline you gave the human, costs money beyond what was agreed) — first one subagent with no chat history: give it links to the goal in the human's words, the task statement, the journal, and what has changed since the last check; name the price and ask it to prove the goal is reached without the extra work, or to find a cheaper path. Accept "not needed" only if it named what confirms the goal without it; "cheaper" — do the cheaper thing; "needed" — a choice with a price, in a separate message to the human at once (see "In the human's language"). The subagent cannot be launched (try first) — ask yourself the same question as a separate step. A step already started turns out to be expensive — say so at once; whether to stop it is the human's call. Record a skipped check and expensive extra work as a line in the task spec or the run journal in `tmp/`: what, what changed and where that is visible, the price, done or not. **The test does not cancel** what the human themselves said "yes" to or asked for, nor the steps the project's rules make mandatory (the review of the task statement, the run after a change, the boundary sample and the reconciliation of the total, the interruption check): the scope the human named is not cut; the scope of mandatory steps is set by question (1). An expansion from another chat without the human's "yes" is extra work: the other chat does not approve growth — it passes the question on to the human. **You are a reviewer** (a gate subagent) — report everything you found: the test is applied by whoever reconciles the findings.

**In the human's language — in every message to them, not only in the final report.** The longer the work runs, the more of your text comes from code, logs, and your reviewers' findings — and the less of it from the human's own words. So once a message is written, read the whole of it before sending and answer four questions. Don't excuse omissions with brevity, or unclear words with precision: the message must be both complete and clear. Something is off — rewrite the message, don't append "let me know if anything is unclear".

- **Am I taking back something I told them earlier? Am I deciding something on their behalf that they will pay for in money, time, or rework?** Then write a separate message, send it at once without waiting for the work to finish, and end it with a question. Open with the retraction itself — "what I told you yesterday was wrong: …" — not with how you found out. As item six inside a long report they will not notice it — which means you did not say it.
- **Does this move what they asked for? What do they gain or lose by it?** The task is the one the work started for, in their words; not your own step and not the latest refinement. Write about it when something changed: closed, stalled, going the wrong way, or only partly closed — then say what is left. Going as it was — stay silent, except in the first message after a long stretch of work. Then say what they gain or lose; count it in money, days, or volume of work. Can't count it quickly — say in one sentence that there is no figure; where there is a figure, don't repeat that caveat. Explain the cause so the human can argue with it: "the program doesn't know how big one piece is", not "the engine refused to convert because of a guard".
- **Is this message enough for them to decide without opening files and without asking again?** Measure completeness against the decision you are asking them to make right now: for that decision say what changes, what problem it closes, what problem it creates, what it costs, and what they are choosing between — in ordinary sentences, not sections with headings. What you decided yourself and are not asking to change — one line for all of it, so the human still has time to stop you. Write the outcome of every reviewer's finding into a file — the gate above requires it; into the message take, in your own words, what changes the human's decision, and a link to the file instead of that content is not allowed. Shorten the language, not the content: drop names and housekeeping detail, but not what they need in order to choose.
- **Is there a word in this message the human never wrote in this chat?** That means a file, function, or folder name; a line, position, or version number; an error, standard, or article code; a name that you or the program gave a thing rather than they did; your working words — lens, task statement, blocker, ingest, maintenance. Look at why the word is there. It names a thing they will look for on their side or use to check you (which object, which document, which line item) — keep it and explain it in one phrase the first time you write it. It explains how the program works inside — drop it, that is for you and not for them. Unsure which half a word belongs to — drop it. Can't remember whether they wrote it, and dropping it is not an option — keep it and gloss it in parentheses.

**Don't throw precision away:** numbers, codes, file names, calculations go to the end of the message under the word "Details", or into a file with the link right there. The human writes in technical words themselves — answer in kind. You are a subagent reporting not to the human but to whoever called you — write precisely, with names and numbers. Don't tell the human you checked your language: clarity shows in the message itself. The human said they didn't understand, or asked you to rewrite — read [methodology/human-language.md](methodology/human-language.md) and rewrite by it.

**A long pass — with progress preserved.** The work runs over a set of items and does not fit into one sitting (a batch of files, a sweep of sources, a series of requests) — the result is written to disk **as it goes**, after each batch rather than at the end: an interruption of the chat, the session, or the machine must leave behind what was done, not zero. Continuation goes by the journal (what has already been processed), what is done is not redone; a failed item goes into the failures list and the pass moves on. The journal, logs, and intermediate chunks live in `tmp/<operation>-<date>/`; **the journal's first line is the task in the human's own words** — once the conversation is compacted there is nowhere left to get them. Once the work is finished and the result accepted, offer cleanup as a list (what gets deleted / what stays), delete on confirmation and never before acceptance. For the project's own code the rule is stricter — see the code mechanic, if it is attached.

**Extraction from a document is a hypothesis, not a reading.** Data lifted from a foreign format (a PDF, a scan, a page's layout, another system's export) is obtained by parsing a **rendering**, not a record: we reconstruct the structure by guesswork — and the guess stays silent when it is wrong, handing back plausible rows instead of an error. It goes wrong **at the boundaries**: a row that starts at the bottom of a page continues on the next one and gets cut in two or glued to its neighbor; the same happens at the seam between batches, at the end of a section, at the pagination edge. So before the bulk run — a sample taken **precisely at the boundaries** (not at the first rows: the middle always parses correctly), and afterwards — **reconciliation of the whole against the source** using something counted independently: the number of records, the sum of a column, the last item. Anything that does not add up, or is unclear, goes into the rejects list and to the human — not into the result as an empty value. Until an extraction has been reconciled, nothing is built on top of it: whatever is built will have to be redone as well. For the project's own code the rule is stricter — see the code mechanic, if it is attached.

**Request resolved — check what was learned and offer to record it in the wiki.** Works without a task too: here, closure means the human's request is resolved. It is resolved when you gave the answer or result that closes it — or the human made it clear the question is settled: "thanks", "it works", "got it", "ok", or simply moved on to something else. Don't wait for words of thanks.

At that point — a short review: what did this conversation turn up that will be needed beyond it and would otherwise have to be figured out again:

1. **The human's decision** — chose among options, agreed with yours, announced it themselves, dropped a path (with the reason). Your recommendation, while they haven't answered, is not yet a decision.
2. **A fact about the project or the outside world** that you will rely on from now on — including one you found yourself while working: a price, a deadline, a contact, how another program or service behaves, a data format.
3. **A confirmed cause of a failure** — non-obvious or in someone else's system.
4. **The human's correction** about how the project works or how things are done here.

Doesn't count: one-off information unrelated to the work; a choice of wording, colour, file name; an edit to the current draft ("shorter"); a typo in what you just wrote; a routine code change; what is already written in the project's files (the file itself is the source); the result of the work itself — the file is already saved, what gets recorded is what was learned along the way; what is already in the wiki and matches (check against the page itself, not the line in `wiki/index.md`); if it contradicts — offer, always and at once.

Found something — as the last line of the text for the human, before "Details", offer to record it: what was learned and why it will be useful, in the human's words, one line for everything. Call the place "project memory" — that is the wiki (`wiki/`), not Claude Code's built-in memory (auto-memory, `MEMORY.md`); on first mention in a chat, explain: "(the `wiki/` folder, visible from any chat)". If the reply ends with a question awaiting the human's decision (an understanding check, a fork, a retraction), move the offer to the next reply, but no later than the final one, even if that ends with a question. Found nothing — say nothing about it.

- **Consent.** Write only after a reply that is about the recording itself: "yes", "go ahead", "save it" to your question about recording; "ok" counts too if the recording question was the last line of your message; a direct request to you to record ("remember this", "save it to memory") is consent as well. The human telling you what they themselves are writing down or dictating ("writing down the inputs: …") is not a request: offer, don't write. An on-topic reply ("we're staying", "we'll take X", a new question) is not consent: don't write. An unanswered offer gets one reminder for the whole chat, in the reply where you wrap up; you may add new facts to it, but don't repeat the question every turn, and without reproach ("you still haven't answered" is not allowed). Declined — don't repeat it in this chat.
- **Where.** Into `wiki/` via ingest ([methodology/ingest.md](methodology/ingest.md), "Knowledge from the conversation"). The "always X / never Y" form goes to `wiki/principles/` (the "Afterwards — capturing the principle" rule below); timing and consent — as here.
- **Reply reviewer — a `.claude/` hook.** In Claude Code your finished reply is also looked at by a separate fast model: if it finds knowledge you didn't offer to record, it sends you back with it. The human already sees the reply: add one separate offer line, don't repeat the reply, and don't talk about the check. If the offer was already made or the human declined — add nothing: reminders follow "Consent" only. Always do your own review: the reviewer is a safety net, not a replacement. Don't mention the consent note that precedes your turn. The human asks to turn the check off — create an empty file `.claude/knowledge-check.off`; to turn it back on — delete it, and if there are no check entries in `.claude/settings.json` — attach it ([methodology/lint.md](methodology/lint.md), "The project-memory check isn't working"): without those entries "turned on" would be untrue.
- **Long work** without replies from the human — findings along the way as a line in the run journal in `tmp/`, to the human — as a list at the end. **You are a subagent** — you don't offer to the human: mark decisions and corrections in your report; a fact from a project file is not a finding.

### Afterwards — capturing the principle

A rule was born — "always X / never Y" — offer to record it in `wiki/principles/<applicability>.md` with its source. Only from concrete cases, never from general reasoning.

---

## Documents and naming

Applies to Claude's working deliverables. Artifacts inside `wiki/` — per [methodology/page-conventions.md](methodology/page-conventions.md).

- **Where things go:** a working file from a task (table, document, report), unless the human named another place, goes into `output/` (into a subfolder where the project's rules say so), and that is not a question for the human: in Claude Desktop the environment's instruction is to put results in the connected folder (simple ones straight into its root); the project's `output/` sits inside that folder, and where exactly inside is decided by the project's rules, not the environment's default. State the path from the project root in your reply. If a file with that name already exists, the task doesn't ask you to change that very file, and it wasn't made in this chat, ask whether to replace it or use a new name. The list: sent from outside, not ours to edit → `raw/`; knowledge → `wiki/` via ingest; working files → `output/` (+ `specs/` for classes with code — <<SLOT S4>>); temporary artifacts of a long pass (progress journal, logs) → `tmp/`, not `output/`; aged-out material from `output/`/`tmp/` that is not worth deleting → `archive/` ([lint.md](methodology/lint.md)).
- **File names.** Descriptive English, underscores; dated when appropriate.
- **Dates.** `YYYY-MM-DD` in file names and YAML; natural English dates or `YYYY-MM-DD` in prose.
- <<SLOT S8: domain conventions — currency and amount format (from the bootstrap interview, a universal question with no default) + units/special citation formats, if any>>
- **Numbers in documents.** Always with a source; without one — `[needs clarification]`.

---

## Bootstrap

No `wiki/`, working layer, or `STATE.md` (or they are partially broken) — Claude follows [methodology/bootstrap.md](methodology/bootstrap.md). Once at initialization; again — only for recovery.
