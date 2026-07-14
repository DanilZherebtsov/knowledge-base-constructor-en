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
