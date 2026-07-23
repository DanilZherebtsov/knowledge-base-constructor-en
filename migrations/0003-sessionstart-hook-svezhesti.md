---
id: 0003
title: "SessionStart hook forcing the session-start checks + STATE hygiene checks"
adr: adr-0032
impact: structural             # a new .claude/ folder appears — can't be expressed by a file swap
applies-to: any               # any assembled instance of any class
requires:                     # input state (from the build fingerprint)
  base: "<=27"
produces:                     # output state
  base: 28
---

# Migration 0003 — SessionStart freshness hook (`.claude/`) + STATE hygiene in lint

> **Executed by the agent INSIDE the instance's live repository.** Applies to any assembled project whose fingerprint carries `base@≤27`. Source of the base skeleton — the public mirror `knowledge-base-constructor-en` (`constructor/base/`, the same channel maintenance uses to check versions by SHA). Rationale — ADR-0032 (hook) and ADR-0033 (STATE checks).

## What changes and the core invariant

`base@28` carries two additive improvements:
1. **`.claude/`** — `settings.json` + `hooks/freshness_check.py`: a `SessionStart` hook that computes the age of the last full `lint` and of `_Updated:_` in STATE and injects deviations (> 7 days) into context at start. It forces the session-start checks that leak when left to prose.
2. **`base@28` prose** — in `CLAUDE.md`, a pointer to the hook (session-start-checks block + discipline §5 + a `.claude/` line in the tree); in `methodology/lint.md`, a "STATE.md hygiene" block (report-only size/drift checks); in `bootstrap.md`, `.claude/` in the template inventory.

> **Migration invariant.** Only the STANDARD layer changes — `.claude/` appears (settings.json + hook), plus the skeleton prose of `CLAUDE.md`, `methodology/lint.md`, `bootstrap.md`, and the build fingerprint. Everything else is byte-for-byte: `wiki/` (page bodies), `raw/`, `STATE.md`, `output/`, `specs/`, `src/`, `data/`, and any hooks/settings already in `.claude/`. Verified by diff (Step 5).

## Step 1. Determine the input state

Read the fingerprint (line 3 of `CLAUDE.md`) — confirm `base@≤27`. Check `.claude/` on disk:
- **Branch A — no `.claude/settings.json`** (folder absent, or it has no settings). → Step 2A.
- **Branch B — `.claude/settings.json` ALREADY exists** (the instance has its own hooks/permissions). → Step 2B (merge, don't overwrite).

## Step 2A. Install `.claude/` wholesale (branch A)

From the mirror (`constructor/base/.claude/`):
1. `base/.claude/hooks/freshness_check.py` → `.claude/hooks/freshness_check.py` (create folders; `chmod +x`).
2. `base/.claude/settings.json` → `.claude/settings.json` (copy as-is — carries no slots).

## Step 2B. Merge the hook into the existing `.claude/settings.json` (branch B) — MERGE

**Do not overwrite the file.** Copy `base/.claude/hooks/freshness_check.py` → `.claude/hooks/freshness_check.py`. In the existing `settings.json`, add the hook object into the `hooks.SessionStart` array (create the key if absent), **without touching** the other keys (`permissions`, other events, any SessionStart hooks already there):
```json
{ "matcher": "startup|resume|clear",
  "hooks": [ { "type": "command",
    "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/freshness_check.py\"" } ] }
```
If such a hook is already present (re-running the migration) — don't duplicate. Idempotent.

## Step 3. Update the STANDARD-layer prose to `base@28`

From the mirror (`constructor/base/`), preserving instance content (filled slots, "About the project", domain sections):
1. `CLAUDE.md` — in the "Operational state" block add the "These checks are forced by a `.claude/` hook…" paragraph; in discipline §5 the tail about the `SessionStart` backstop; in the "Architecture" tree the `.claude/` line.
2. `methodology/lint.md` — into the "## Report-only" block add the "### `STATE.md` hygiene" subsection (6 report-only checks; thresholds ~2000 chars / ~30 KB / ~10 KB / 7–14 days).
3. `methodology/bootstrap.md` — add `.claude/` to the "template already ships with …" list.

> If the instance carries the `spec-lifecycle` mechanic, maintenance will separately pull `spec-lifecycle@6` (the "Spec status drift" bullet into the same "Report-only" block) — that is a routine `[instruction]` part update, not part of this migration.

## Step 4. Update the build fingerprint

Line 3 of `CLAUDE.md`: raise `base@N` → `base@28`. Other parts unchanged.

## Step 5. Verify

- `git diff` — only `.claude/`, `CLAUDE.md`, `methodology/lint.md`, `methodology/bootstrap.md`, and the fingerprint are touched. `wiki/`, `raw/`, `STATE.md`, working layers — untouched.
- Hook is executable: `CLAUDE_PROJECT_DIR="$PWD" python3 .claude/hooks/freshness_check.py` — when maintenance is overdue it prints JSON with `additionalContext`; when fresh it stays silent; always `exit 0`.
- **Maintenance** after the migration: the part comparison against upstream shows `base@28`; no more updates from this transition are available.
