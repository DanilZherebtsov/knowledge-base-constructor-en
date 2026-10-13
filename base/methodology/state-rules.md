# STATE.md — structure and update rules

`STATE.md` at the root is the single place for current plans and progress. The wiki stores knowledge (append-mostly, sourced, atomic); STATE stores intentions (frequent rewrites, no sources, everything in one file for at-a-glance review).

## Structure

A fixed set of sections, always in the same order. Empty ones stay, marked `_empty_`, and are not deleted (the eye gets used to the layout).

<<SLOT S5: the class's section list. Generalized default: Stage / Path to goal / In progress now / Next / Completed last week / Blockers and risks / (a domain 7th). A class may rename/rethink (example: business — Snapshot / Open tracks / This week / Next / Commitments calendar / Blockers, risks, decisions / Recently completed).>>

**Granularity mirrors the horizon.** Early on — broad strokes; as things draw closer, they break down into detail. Do not atomize prematurely (false precision).

## Update rules

1. **Source of truth.** STATE is the single place for the current plan. No parallel lists.
2. **Claude updates at three moments:** (a) silently reads it at session start; (b) after an item is finished, moves it into the "completed" buffer; (c) after an ingest that shifts priorities — proposes an edit, the human confirms.
3. **The human updates at two:** a change of priorities; a new blocker. Via chat or by hand.
4. **Freshness by date.** The `_Updated:_` field is mandatory. The session-start freshness trigger (what to do when it is stale) lives in CLAUDE.md (always-on); it is not duplicated here.
5. **Link to the wiki with markdown links.** An item that references knowledge does so via `[Title](wiki/type/slug.md)`. The knowledge lives in the wiki; STATE holds the phrasing and the link.
6. **The "completed" buffer.** An item sits for a week, then collapses: one that produced a wiki page → a link to it; under git, routine with no trace → a line "N tasks over the period …, see git log"; without git — deleted. The memory of work lives in wiki/git, not in STATE.
7. **STATE is not canonical.** On conflict with the wiki/code, the truth is there. Lint may flag a divergence, never fixes it automatically.

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
