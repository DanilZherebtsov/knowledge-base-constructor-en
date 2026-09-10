---
id: 0004
title: "The whole move to base@43: the reply reviewer and the consent note (.claude/) + the other parts of base@43"
adr: adr-0054
impact: structural             # merging entries into someone else's settings.json — not a file swap
applies-to: any                # any assembled instance of any class
requires:                      # input state (from the build fingerprint)
  base: "42"
produces:                      # output state
  base: 43
---

# Migration 0004 — the reply reviewer, the consent note and the whole move to base@43

> **Executed by the agent INSIDE the instance's live repository** whose fingerprint carries `base@42`. This is the **only path** from 42 to 43: this migration also carries the other parts of base@43 (all other blocks of the `## base@43` changelog entry) — step 4. Do not adopt base@43 by the changelog headings: update check compares version numbers, and a version adopted halfway will never catch up with its other half. Source — the public mirror (`constructor/base/`). Knowledge — ADR-0054 (the reviewer) and the ADRs named in the other blocks.

## What changes, and the main invariant

1. **`.claude/`** — `hooks/consent_nudge.py`, `hooks/judge.py` and two entries in `settings.json`: `UserPromptSubmit` (the consent note before every agent turn) and `Stop` (a separate fast model looks at the finished reply; if it finds knowledge the agent didn't offer to record, it sends the agent back to add one offer line). The reviewer leaves its trace in `tmp/knowledge-check/`.
2. **Prose** — the "Request resolved" rule and the `.claude/` tree line in `CLAUDE.md`; the "Knowledge from the conversation" section in `methodology/ingest.md`; `methodology/bootstrap.md`; the "The project-memory check isn't working" check in `methodology/lint.md`; `HELP.md` lines — with the human's "yes" (step 6).
3. **The other parts of base@43** — by the "How to adopt" of their changelog blocks.

> **Invariant of the migration.** Only the STANDARD layer changes: `.claude/`, `.gitignore` (the `tmp/` line, if missing), `CLAUDE.md`, `methodology/*.md`, `HELP.md` (with a "yes"), the fingerprint. `wiki/` (page bodies), `raw/`, `STATE.md`, working layers, other hooks and keys in `.claude/` — byte for byte. Checked by diff (step 5). **Every step is idempotent:** what is already done is not duplicated; an interrupted migration can simply be run again.

## Step 1. Input state

Fingerprint (line 3 of `CLAUDE.md`) — `base@42`; otherwise first catch the feed up to 42. Then:
- `.claude/settings.json` exists → branch B; missing (the human removed the folder) → branch A.
- `disableAllHooks: true` in `settings.json` or `settings.local.json` → one sentence to the human: "automatic checks are switched off in this project — the project-memory check won't work"; don't touch the flag, carry on.
- `command -v claude` finds nothing → don't add the `Stop` entry (the reviewer), only `UserPromptSubmit`; one sentence to the human: "checking my replies isn't available — there's no `claude` command; I'll still offer to save what matters, but less often".

## Step 2A. No `settings.json`

Copy the files as in 2B, then create `.claude/settings.json` with only the entries of step 2B. Don't restore the freshness hook (`SessionStart`) without asking — the human removed it.

## Step 2B. Merge into the existing `settings.json` — a MERGE

Copy `base/.claude/hooks/consent_nudge.py` and `base/.claude/hooks/judge.py` from the mirror → `.claude/hooks/` (`chmod +x`). **Do not overwrite the file:** append one object each to the `hooks.UserPromptSubmit` and `hooks.Stop` arrays (create the keys if missing), leaving other keys and existing hooks untouched:

```json
{ "hooks": [ { "type": "command",
    "command": "command -v python3 >/dev/null 2>&1 && python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/consent_nudge.py\" || true" } ] }
```

```json
{ "hooks": [ { "type": "command", "timeout": 90,
    "command": "command -v python3 >/dev/null 2>&1 && python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/judge.py\" || true" } ] }
```

A command with `consent_nudge.py` / `judge.py` is already there (in `settings.json` or `settings.local.json`) — don't duplicate.

## Step 3. The reviewer's prose

From the mirror, keeping the instance's content (filled slots, "About the project", domain sections). Find places by headings and text, not by line numbers; what is already inserted — don't duplicate:
1. `CLAUDE.md` — the "Request resolved — …" block right before `### Afterwards — capturing the principle`; in the "Knowledge synthesis on closing a unit of work" item, after "Skipping this = the wiki falls behind what we actually know." — the sentence about "Request resolved" (don't touch the item's tail with the slot — the class mechanic filled it); in "It grows through" — "(e) …"; replace the `.claude/` tree line.
2. `methodology/ingest.md` — the pointer after the "Triggered by phrases…" line; the addition to "**Same thesis**"; the "Knowledge from the conversation" section — at the very end of the file, after all sections, including ones added by mechanics. Don't replace the file wholesale.
3. `methodology/bootstrap.md` — the `.claude/` description.
4. `methodology/lint.md` — the "The project-memory check isn't working" item in "Report-only".

## Step 4. The other parts of base@43

All other blocks of the `## base@43` changelog entry — each by its own "How to adopt", except the lines for `HELP.md`: those go to step 6. What is already done — don't duplicate.

## Step 5. Check

- `git diff` — only `.claude/`, `.gitignore`, `CLAUDE.md`, `methodology/*.md`. `wiki/`, `raw/`, `STATE.md`, working layers — untouched.
- `settings.json` is valid JSON; `python3 .claude/hooks/consent_nudge.py < /dev/null` prints one line; `echo '{}' | python3 .claude/hooks/judge.py` — silent, exit code 0.
- The reviewer itself starts working from the next session: hooks are read at startup.

## Step 6. `HELP.md` — one question to the human

The cheat sheet is the human's to edit. Show all new base@43 lines in one question, without jargon: the "Chat context" item (as a replacement) and "The project-memory check gets in the way" from this part, plus the lines from the other `## base@43` blocks. Add what they said "yes" to (no suitable section — at the end of the file). Declining all or some — don't add, a dated line in `wiki/log.md`; the migration counts as complete.

## Step 7. Fingerprint

Line 3 of `CLAUDE.md`: `base@42` → `base@43`. Other parts — unchanged.

## Rollback

Turn the check off without rolling back the version — an empty file `.claude/knowledge-check.off` (to the human: "turn off the memory check"). Remove it entirely — delete both entries from `settings.json` and the files `consent_nudge.py`, `judge.py`; the "Request resolved" rule stays and works on its own, more weakly.
