# STATE.md — structure and update rules

`STATE.md` at the root is the single place for current plans and progress. The wiki stores knowledge (append-mostly, sourced, atomic); STATE stores intentions (frequent rewrites, no sources, kept short — it is read at the start of every chat).

**STATE holds the current and the near-term, the path to the goal, and commitments.** Anything "for later" in it is a tax on every chat: deferred items go to `BACKLOG.md` (the section below). Cleaning STATE is not a reason to write into the wiki: the details of a current item go into the unit of work (a spec, a decision file), those of a deferred one — into BACKLOG, and knowledge reaches the wiki by its own routes (ingest, "Knowledge synthesis on closing a unit of work" in "Discipline", "Request resolved").

## Structure

A fixed set of sections, always in the same order. Empty ones stay, marked `_empty_`, and are not deleted (the eye gets used to the layout).

1. **Stage** — where we are and what the current goal is: for a product — idea / prototype / MVP / beta / v1; for a business — the stage of the business, key numbers with a source. Not a plan — the current picture.
2. **Path to goal** — milestones from the current moment to the goal, in order. Parallel tracks (open a location, launch a product) — as separate items, each with its own outcome and a link to the key decisions in the wiki.
3. **In progress now** — what is being worked on right now (usually one or two tasks), with the path to the working file: a spec in `specs/` (`status: active`), a decision's comparison file. For a sprint — a link to the sprint spec (`SPRINT-<NAME>.md`) and the active task; the sprint's task list is not duplicated here. Only what is actually being worked on now.
4. **Next (1–2 weeks)** — only what we are certain to take up in the next two weeks; one-liners, the file is created when the task is taken up. Beyond the horizon — into "Path to goal"; deferred — into `BACKLOG.md`.
5. **Commitments calendar** — recurring and dated commitments (rent, taxes, renewals, reporting): due date and cadence; how it is kept — the section below. No commitments — `_empty_`.
6. **Blockers, risks, and decisions waiting on me** — what is in the way (a blocker), what could get in the way (a risk and a plan B), what awaits the human's word (with a link to the working file or `synthesis/`). Everything that awaits their word lives here, not in `BACKLOG.md`; risks outside the current work — in `BACKLOG.md`.
7. **Recently completed** — the buffer (rule 6 below).

Tech debt — the "Tech debt" section of `BACKLOG.md` (links to ADRs with status `active` and tag `tech-debt`), not STATE.

**Granularity mirrors the horizon.** Early on — broad strokes ("shape the concept", "10 interviews", "build the MVP"); as things draw closer, they break down into detail. Do not atomize prematurely (false precision).

## Update rules

1. **Source of truth.** STATE is the single place for the current plan, `BACKLOG.md` — for deferred items. There are no other parallel lists.
2. **Claude updates at three moments:** (a) silently reads it at session start; (b) after a significant item is finished, moves it from "In progress now" to "Recently completed" with today's date; an ingest or a decision added a commitment (a lease, a tax regime, a hire) — proposes a line in the "Commitments calendar"; (c) after an ingest that shifts priorities — proposes an edit, the human confirms. A deferred item appeared — straight into `BACKLOG.md`, not into STATE.
3. **The human updates at two:** a change of priorities; a new blocker. Via chat or by hand.
4. **Freshness by date.** The `_Updated:_` field is mandatory and is written as a line of its own that starts like this: `_Updated: YYYY-MM-DD_` — ISO date only, nothing but the date between the underscores. A note, if needed, is one short phrase after the closing `_`: with a phrase inside the underscores, or with a spelled-out date, the session-start check finds no date and stays silent, as it would with a fresh STATE. The session-start freshness trigger (what to do when it is stale) lives in CLAUDE.md (always-on); it is not duplicated here.
5. **Link to the wiki with markdown links.** An item that references knowledge does so via `[Title](wiki/type/slug.md)`. The knowledge lives in the wiki; STATE holds the phrasing and the link; the details of a current item live in the unit of work, those of a deferred one — in BACKLOG. A unit of work's working file in `output/` is named in the item by its path — that is how maintenance sees the file is alive.
6. **The completed buffer** (the "Recently completed" section). An item is a list entry: the date it was moved into the buffer, what was done, a link. Maintenance itself deletes items older than 7 days counted from `_Updated:_`, and of those that remain — everything beyond the ten most recent ([lint.md](lint.md)). The memory of work lives in the units of work and the wiki; in git — if STATE was committed; otherwise what is deleted is kept nowhere.
7. **STATE is not canonical.** On conflict with the wiki/code, the truth is there. Lint may flag a divergence but does not fix it itself (in STATE, maintenance itself only cleans the buffer — rule 6).

## `BACKLOG.md` — deferred items

A file in the root for what we are not doing now and are not taking up in the next two weeks: "someday" items, tech debt, things started and dropped, risks outside the current work. Everything that awaits the human's decision lives in STATE ("Blockers, risks, and decisions waiting on me"), not here. It is not read at the start of a chat.

- **Not here — an item with a due date.** A due date is a date by which, or after which, the item must be raised: a deadline, "after DD Mon", a recurrence; the date an item was written down or deferred is not a due date. The file is not read at session start, so the date would be missed; such an item stays in STATE as a line with its due date: a commitment — in the "Commitments calendar"; anything else — in "Path to goal" or "Next". The tail of the current task doesn't go here either, until the human has given it an outcome.
- **Created with the first deferred item**, never created empty. A file with this name already exists and has no header as below — it is not ours: ask the human where to keep deferred items. "Keep them here" — add the header; they named another file — replace `BACKLOG.md` with its name in `CLAUDE.md` (the "Architecture" tree and "Operational state"): the name of the file for deferred items is always taken from there.
- **Header:** `# BACKLOG — deferred (not in progress)` and the line "Not read at the start of a chat. When an item is taken into work, its line moves to STATE and is deleted here. Not canonical."
- **Sections** — as needed, with fixed names: "Started and dropped", "Someday", "Tech debt", "Open questions", "Risks". An item is a phrasing, the date it was deferred, and a link (unit of work, wiki, working file).
- **When to read it:** a milestone or a unit of work has closed, or "Next" is empty — we are choosing what comes next; a question about plans beyond two weeks; "what's in the backlog", "what about tech debt"; taking up a task in a new area — look through "Risks", "Tech debt", "Started and dropped" on its topic.

## Commitments calendar — how it is kept

STATE's section 5 is not a task list but a list of **commitments**: things that recur or have a hard external deadline with consequences for missing it (a fine, penalties, a broken contract, a revoked license). A task is done once and closed; a commitment cannot be "completed" — only serviced until the next cycle.

**What is not here.** The calendar is not moved into `BACKLOG.md`. An item with a due date that is not a commitment ("come back to the question after 14 Oct") is a line with its due date in "Path to goal" or "Next".

**What goes in.** Rent, taxes and contributions, payroll and regular payouts, renewal of contracts / licenses / domains / insurance, periodic reporting, recurring payments to suppliers.

**Line format:** what — due date and cadence — amount (with a source) — link to the related entity/decision/document.

```text
- Rent (Main St) — monthly on the 5th, $2,400 → [landlord-main-st](wiki/entities/landlord-main-st.md)
- Quarterly tax — quarterly, next due 2026-07-28
- Domain renewal — annually, by 2027-03-14
```

**How it is replenished.** The human adds entries manually; Claude — by rule 2b: an ingest or a decision produced a recurring commitment → proposes a line, taking the due date and amount from the source, never inventing them.

**Reminder threshold.** A commitment within 7 days, or an overdue one, Claude raises in the session-start (silent) check; maintenance additionally names near and overdue ones in its report.

**On fulfillment.** A recurring one does not move to "Recently completed" — Claude updates the next due date to the next cycle. A one-off is removed from the calendar after fulfillment.

**This is a list, not automation.** The reminders are made by Claude while reading, not by a background process.

## Handoff to a new chat

The human asks to continue the work or take up a new task in another chat ("hand this off to a new chat", "give me a handoff", "dump the context for a new chat") — write down what is needed so that a chat without this conversation can do the work. A summary the human is going to read is not a handoff but a result: it goes to `output/`.

**Where.** The human named a place, asks for the text right in the reply, or said they will continue outside this folder (another machine, a cloud session) — do that; in the last case give the handoff text in the reply: a file won't get there. The task's file, if there is one, gets updated anyway. Otherwise, without asking:

- **The task already lives in its own file** (an active spec, a question's working file, a comparison file) — add a "Handoff <date>" section to it: where we stopped, what was tried and rejected, the next step, what was decided along the way. No separate file is needed: decisions about a task live with the task, not in a temporary file.
- **A new task of the kind the project opens a spec for** (a feature, a fix; `specs/` in the "Architecture" tree of `CLAUDE.md`) — open a spec with what is already known: the task in the human's words, what has been decided, what is out of scope — by the spec's rules (`status: active`, a line in `STATE.md`), and mark in it: the task statement is not written and not reviewed yet. The task statement and its review are done by the chat that continues. "Look into it", "check", "finish looking at", "dump the context" — not a spec; in doubt — the next item.
- **Everything else** — `tmp/handoff-<topic>-<date>.md`. The `handoff-` prefix is always exactly that: it is how a handoff is recognized. Name taken — add a number. Under git — before writing, make sure `tmp/` is in `.gitignore`; if not — add it.

In the first two cases, offer to record in the wiki right away, in this chat, whatever was learned that will be needed after the task (`CLAUDE.md`, "Request resolved"): there is no separate handoff file to carry it.

**What goes into the handoff file or text:**

1. First line — the task in the human's words.
2. Continuing work — where we stopped, what was tried and rejected and why, the next step. A new task — what to do, what is already decided, the acceptance criteria. Don't retell the plan from `STATE.md` or the unit of work — link to them.
3. Files — as links from the project root, not copies. This chat was itself started from a handoff — carry its rejected attempts and its "Not yet saved to project memory" section over into the new one rather than linking: that handoff is temporary.
4. A "Not yet saved to project memory" section — the human's decisions (in their words, with the date) and facts that are not in `wiki/` and will be needed after the task. What was recorded in the wiki right away doesn't go here. Empty — say so. A handoff is temporary: its contents do not count as "already written in the project's files".
5. Last line: "When you finish the task — offer to record in project memory (`wiki/`), for the human, whatever from the 'Not yet saved to project memory' section isn't there yet, and note here what they answered. Don't delete this file: maintenance will name it." In a text for the reply and in a section of the task's file — without the second sentence.

No secret values in a handoff — only the name and where it lives ([secrets-rules.md](secrets-rules.md)).

**To the human, in the reply** — the path from the project root and a phrase for the new chat with the real file name (the task's or the handoff's): "read `<path to the file>` and continue".

**Why item 5.** A chat started from a handoff doesn't open this section (and in Claude Desktop may not see `CLAUDE.md` either): the file itself is what guides it. It doesn't offer deletion because in Claude Desktop a deletion needs a permission that opens the whole connected folder until the end of the chat — not something to ask for over one file; maintenance names handoffs left lying around all at once ([lint.md](lint.md)), and deletion happens only on the human's "yes".
