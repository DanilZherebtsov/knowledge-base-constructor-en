---
id: 0007
title: "Moving a 2.x project to 3.0.0 — one project for all programs"
adr: adr-0069
impact: structural             # the main file CLAUDE.md → AGENTS.md; the checks .claude/hooks/ → hooks/
applies-to: any
requires:
  version: "2.x"               # any 2.x; a 1.x project goes through 0006 first
produces:
  version: 3.0.0
supersedes: []
---

# Migration 0007 — moving a 2.x project to 3.0.0

> **Executed by the agent INSIDE a 2.x project**, during maintenance: by the "How to adopt" of the journal's `## 3.0.0` entry, or by step 0 of the new `lint.md`'s "Update check" if the move was interrupted. A 1.x project first goes through [0006](0006-perehod-na-2-0-0.md), and this move comes at the next maintenance, in a new chat. Read this file from the tip of the mirror's `main` branch (it has the latest fixes); take the 3.0.0 files **at the tag `v3.0.0`**, the merge base — **at your own version's tag** `v<line 3 of CLAUDE.md>`. Knowledge — ADR-0069.

## What changes and the main invariant

As of 3.0.0 one and the same project works in Claude Code, Codex and Qwen Code:
- **the rules** — in `AGENTS.md`: the former `CLAUDE.md` is renamed, the project's own edits in it are kept by merging; `CLAUDE.md` is one line `@AGENTS.md`, through which Claude Code reads the rules;
- **the project's checks** — shared scripts in the visible `hooks/` folder instead of `.claude/hooks/`; the settings of three programs connect them: `.claude/settings.json`, `.codex/hooks.json`, `.qwen/settings.json`;
- **the programs' specifics** — `methodology/environment/`; the rules and the help name no programs.

**Before the human's "yes", nothing is written into the project** (steps 1–4), except committing this same run's auto-fixes (step 1.4) and `disableAllHooks` with the human's consent (step 1.6); the analysis and drafts — in a temporary folder outside the project.

> **Invariant.** After the "yes", 0007 writes only:
> - the paths of the `v3.0.0` tree (the closed list, step 2.3);
> - check entries in personal settings files — only by a replacement shown to the human, with their "yes" (step 6.4);
> - it removes only the canon's old scripts in `.claude/hooks/` (step 6.5) and moves the reply-check switch (step 6.6);
> - the move lines in `wiki/log.md` (literals below), the progress file, the move commit;
> - the folder `tmp/knowledge-check/` with its own `.gitignore` — the new checks create them themselves on their first run.
>
> Everything else — byte for byte: the bodies of `wiki/`, `wiki/index.md`, `raw/`, `input/`, `output/`, `specs/`, code, `tmp/`, `archive/` (under git), `STATE.md`, `BACKLOG.md`, `CLAUDE.local.md`, the project's own hooks and settings keys, its own files named like the mirror's service files. Temporary material (the mirror clone, the analysis) — **only outside the project**: a foreign rules file read inside the project would be applied by the program as instructions.

**Journal literals** — lines `## [YYYY-MM-DD] update | …` in `wiki/log.md`, matched by the exact text after `| `:
- `move to 3.0.0 — started at <SHA>` (without git — `started without git`) — written after the "yes";
- `move to 3.0.0 — done`;
- `move to 3.0.0 — rolled back (<why>)` — a technical rollback, not the human declining: no pause;
- `move to 3.0.0 — declined` — offer again no sooner than 30 days later;
- `move to 3.0.0 — declined for good` — don't offer until the human asks; the project stays on 2.x;
- `can't`.

**Progress file** — created after the "yes", in the project: `tmp/0007/hod.md` (not in `.git/`: programs treat `.git/` as a protected folder and refuse writes there; under git `tmp/` is not committed). In it: "yes received <date>", the start SHA, your version, the closed list, the human's decisions on the screen, the closed-list paths that did not exist at the start, a mark after every item of step 6.

**The move is complete** only when line 3 of `AGENTS.md` = `3.0.0`, `CLAUDE.md` is exactly `@AGENTS.md`, the journal has `move to 3.0.0 — done`, and (under git) the move is committed.

**Before any question to the human** — one sentence of substance: "an update is out: the project now works not only in Claude Code but also in Codex and Qwen Code; for that the rules move into the `AGENTS.md` file and the automatic checks into the `hooks/` folder. I can move the project over; first one clarification: …".

## Step 1. Entry (in order, no writes)

1. **Journal and progress.**
   - There is `— done` and line 3 of `AGENTS.md` = `3.0.0`: under git the move isn't committed — steps 8.3–8.4, otherwise 8.4; stop.
   - The progress file marks "8.1" and line 3 of `AGENTS.md` is still the old one — a break in step 8: run step 8 from item 1 (add the `— done` line if it is missing).
   - There is `— started` with no later `— done` / `— rolled back…` / `— declined…` and a progress file with "yes received" — ask "the move was interrupted on <date>: shall I continue, or put everything back as it was?"; "put it back" — rollback (step 7, "Rollback") and the line `— declined`; "continue" — continue: under git — if the start (the SHA in the `— started` line) is an ancestor of `HEAD` (`git merge-base --is-ancestor <start> HEAD`) and the commits after it touched only closed-list paths, `.claude/` and `wiki/log.md` (otherwise — show the human what other paths changed and ask). Continue from the first unmarked item of step 6 (your version — from the progress file; don't check precondition 1.5: `AGENTS.md` and `hooks/` are already ours); the clone and the analysis — anew (steps 2–3), the old versions — from `git show <start>:<path>` (without git — from `archive/0007/before.tar`). No progress file, or no "yes received" in it — start from step 2.
   - The last line about the move is `— declined` younger than 30 days: don't offer; the maintenance verdict — "mine is <version> — the mirror has 3.0.0; the move was postponed by the human on <date>". `— declined for good` — don't offer until the human asks.
   - Otherwise — step 1.2.

   An unfinished move goes first in a maintenance pass: its other items (auto-fixes included) — after it.
2. **Git, shell, mirror.**
   - No git (`git rev-parse` fails) — the "without git" path (steps 5 and 7).
   - No shell or no access to the mirror — the sentence of substance and a request to open the project where the move will go through: in Claude Code — the terminal, VS Code or the Code tab (not a Claude Desktop project); in Codex — with network access allowed. Write `can't` only if the human answered that they can't or won't; in such an environment don't offer the move for 4 weeks.
3. **The repository** (`git rev-parse --path-format=absolute --show-toplevel --git-dir --git-common-dir`) — as in step 1.3 of migration 0006: the project lies inside a bigger repository — say so and ask; **a shared repository** (more than one human author; treat a shallow clone as shared) — the move goes on a separate branch for an MR from the branch where the project merges rule changes; **a single author** in a git worktree — don't move, name the main folder; there is an `origin` — `git fetch`: on the branch where rules are merged, line 3 of `AGENTS.md` is already `3.0.0` — say "merge <branch> into yours", stop.
4. **Uncommitted changes** — as in step 1.4 of migration 0006: a merge or rebase in progress — wait; this run's auto-fixes — commit them (`lint: auto-fixes before the move`); the human's uncommitted edits in closed-list paths, `.claude/`, `wiki/log.md` — don't commit them yourself: say so and wait.
5. **Something else in place of the new paths.** The root already has:
   - `AGENTS.md` — it isn't from the constructor (a project with code, another program's rules): **stop**, tell the human and give a choice — rename it (whatever is needed from it is carried into the project's rules later, by a question), keep the project on 2.x (`— declined`), or postpone;
   - a `hooks/` folder without the constructor's `hooks/common.py` — **stop** and the same choice (where to move their folder);
   - `.codex/` or `.qwen/` — not a stop: their files are not overwritten, the new content is merged by entries (step 6.4).
6. **Other hooks that commit or push** — as in step 1.5 of migration 0006: offer `"disableAllHooks": true` in `.claude/settings.local.json` for the duration of the move and continue in a new chat; they don't agree — don't start the move. The project rule "commit or push after every reply" is not applied until step 8 — say so in one line.

## Step 2. Snapshot (outside the project)

1. **Clone the mirror** — at the address from step 2 of your `methodology/lint.md`'s "Update check"; outside the project, a full clone:
   ```bash
   D=$(mktemp -d) && git clone -q <mirror address> "$D" && git -C "$D" checkout -q v3.0.0
   ```
   No `v3.0.0` tag — to the human: "the new release of the constructor isn't fully out yet, I'll come back to it at the next maintenance"; don't write `can't`, stop.
2. **Your version** — line 3 of `CLAUDE.md` (`> **Version:** 2.x.y`; when continuing — from the progress file); `git -C "$D" rev-parse -q --verify "refs/tags/v<my version>"` — no tag: don't move, say it plainly ("I couldn't find in the constructor the version this project started from — I can't move it without touching your edits"), as a line in `wiki/log.md`, stop.
3. **The closed list** — the `v3.0.0` files that end up in the downloaded archive:
   ```bash
   git -C "$D" -c core.quotePath=false ls-tree -r --name-only v3.0.0 \
     | while IFS= read -r p; do git -C "$D" check-attr export-ignore -- "$p" | grep -q ': set$' || printf '%s\n' "$p"; done
   ```
4. **What was renamed and what was removed** — without guessing renames:
   ```bash
   git -C "$D" -c core.quotePath=false diff --no-renames --name-status "v<my version>" v3.0.0
   ```
   The correspondences — by this table, not by text similarity:

   | was (2.x) | became (3.0.0) |
   |---|---|
   | `CLAUDE.md` | `AGENTS.md`; a new `CLAUDE.md` = `@AGENTS.md` |
   | `.claude/hooks/freshness_check.py` | `hooks/start.py` |
   | `.claude/hooks/consent_nudge.py` | `hooks/consent.py` |
   | `.claude/hooks/judge.py` | `hooks/judge.py` |
   | `.claude/hooks/raw_guard.py` (since 2.2.0) | `hooks/raw_guard.py` |
   | — | `hooks/common.py`, `hooks/env_check.py`, `methodology/environment/`, `.codex/`, `.qwen/` |
   | `.claude/knowledge-check.off` | `.knowledge-check.off` or `tmp/knowledge-check/claude/off` (step 6.6) |

## Step 3. What is yours (analysis outside the project)

1. **Closed-list files present in the project** (except `AGENTS.md`, `CLAUDE.md`, the programs' settings files and `hooks/` — items 2–4) — three versions into a temporary folder: my file, the base `git -C "$D" show "v<my version>:<path>"`, the new one `git -C "$D" show "v3.0.0:<path>"`; then `git merge-file -p <mine> <base> <new> > <result>`. Exit code 0 — the result is ready; a conflict — work it out by meaning and put it on the "asking" screen in plain words ("the update changed X, you have your own Y here — keep yours and add X?"). A closed-list file the project doesn't have: it was there at your version's tag — the human deleted it: onto the "asking" screen; it wasn't — it is new and goes in as is.
2. **The main file** — three versions: my `CLAUDE.md`, the base `v<my version>:CLAUDE.md`, the new `v3.0.0:AGENTS.md`; cut the "About the project" section and line 3 out of all three before merging, afterwards put "About the project" back verbatim and line 3 as it was (step 8 changes it). Expect more conflicts here than usual: 3.0.0 removed program names from the rules — work them out the same way, by meaning.
3. **Scripts.** The canon's four old scripts in `.claude/hooks/` (the table in step 2.4) — compare with `v<my version>`: equal — it will be removed (step 6.5); different — name the human's edits on the "asking" screen (whether and how to carry them into the new script — by meaning). The human's own scripts in `.claude/hooks/` are not touched.
4. **Check entries in all of Claude Code's settings files** — `.claude/settings.json`, `.claude/settings.local.json`, `~/.claude/settings.json` (read only): every command that points to an old canon script in `.claude/hooks/` — a replacement plan: path → `hooks/`, the script name — per the table in step 2.4, `--program claude` at the end of the script call; the rest of the human's command form (another `python`, a Windows form, their own keys) is not touched. A command exactly as in `v<my version>` — replace it whole with the `v3.0.0` command, the `SessionStart` `matcher` — as in `v3.0.0`. A check entry the human doesn't have but their version did (they removed it) — don't bring it back; a `v3.0.0` entry their version didn't have (the `raw/` guard — before 2.2.0) — add it, unless declined in the journal. The journal lines `setup | hooks: not adding`, `setup | raw/ guard: not adding`, `setup | style: …` apply to all programs: don't add the matching entries in `.codex/` and `.qwen/` either (item 6.4) and say so on the screen.
5. **The reply-check switch** — there is a `.claude/knowledge-check.off`: in `tmp/knowledge-check/log.txt` the last lines before it appeared are three `ERR` in a row → it turned itself off after failures → `tmp/knowledge-check/claude/off`; otherwise it is the human's request → `.knowledge-check.off` in the root. Unclear — ask on the screen.

The result of the analysis — in a temporary folder outside the project (`$D-razbor.md`).

## Step 4. One screen, one "yes"

One message to the human, in plain words, without the words "version, tag, merge, migration, hook" (and the other forbidden words of your `lint.md`). Three groups:
1. **What changes** — the substance and, necessarily, by action:
   - the project will work in Codex and Qwen Code too; the rules — in `AGENTS.md`, next to it a one-line `CLAUDE.md` — no need to delete it;
   - the automatic checks move into the `hooks/` folder;
   - **in Claude Desktop** (if the project is open there) — the line in the project instructions will need replacing: I'll give the new one;
   - **in Codex and Qwen Code** (if you work there) — turn on the reminders, the saving prompt and the protection of the source documents there: I'll tell you how.
2. **What is yours and where I carry it** — briefly, per the analysis.
3. **What I'm asking** — conflicts, files you deleted, edited scripts, the exact replacement in personal settings files (I write it with your "yes", or you do), the switch.

And the question: "shall we move? You can also answer 'not now' or 'don't offer again'".
- **"Yes"** — step 5.
- **"Not now" / "no"** — `move to 3.0.0 — declined`, stop. **"Don't offer again"** — `move to 3.0.0 — declined for good`. In both cases delete the temporary material outside the project; if hooks were turned off in step 1.6 — remind them to remove `disableAllHooks`. A shared repository: write the decline line only if the person answering decides on the project's rules.

## Step 5. Start (after the "yes")

0. **A shared repository** — create the branch for the MR from `HEAD` now; the start = its tip.
1. The journal line `move to 3.0.0 — started at <SHA HEAD>` (without git — `started without git`).
2. The progress file: "yes received <date>", the start SHA, your version, the closed list, the human's decisions, the closed-list paths **that don't exist at the start**.
3. **Under git — the "before" status** into the progress file: `git -c core.quotePath=false status --porcelain -uall --ignored=matching`. Untracked and ignored closed-list paths, `CLAUDE.md`, `.claude/settings.local.json`, `.claude/hooks/` and `.claude/knowledge-check.off` — copied into `tmp/0007/before/` (the rollback restores them from there).
4. **Without git:** a copy of the closed-list paths present in the project, of `CLAUDE.md` and of the whole `.claude/` folder — as the archive `archive/0007/before.tar`; an inventory of the rest (path, size, mtime) — in `archive/0007/inventory.txt`; there is an `archive/index.md` — a line about the `0007/` folder.

## Step 6. Writing — strictly in this order

A break after any item leaves the project working: the old rules and checks act until item 8, and the new `lint.md` from item 1 brings you back here (step 0). After each item — a mark in the progress file.

1. **`methodology/lint.md`** — the merge result (step 3.1).
2. **The other closed-list files** — except `AGENTS.md`, `CLAUDE.md`, `hooks/` and the programs' settings files: the merge results; new ones (`methodology/environment/` and others) — as is. Paths under `wiki/`, `raw/`, `input/`, `output/` (stubs) — only if the project has no such folder.
3. **`hooks/`** — the six scripts from `v3.0.0` byte for byte, `chmod +x`; the human's edits from the old scripts — per their decision.
4. **The programs' settings files:**
   - `.claude/settings.json` — by entries, per the plan of step 3.4 (their own keys and entries — byte for byte; the `outputStyle` entry — as it was);
   - `.claude/settings.local.json` — the same replacement, only with a "yes" on the screen; `~/.claude/settings.json` — outside the project: write the shown replacement only on the human's direct request, otherwise they write it themselves — until then the old scripts their commands point to are not removed (item 5);
   - `.codex/`, `.qwen/` — from `v3.0.0`; already present at the human's — by entries and keys, their own — byte for byte; entries the human declined (step 3.4) are not added;
   - the `.claude/settings.json` write didn't go through (the program's protection, automatic mode) — the "Writing the checks and program settings" section of the new `methodology/lint.md` and the environment file `methodology/environment/claude-code.md`. **Don't run items 5–8 before it:** without it the old commands point to the old scripts. The mark "6.4 waiting" in the progress file, a question to the human; written (the program let it through after permission, or the human wrote it by hand) — on to item 5; "don't add them" — the move can't finish without this edit: say so and ask whether to put the project back as it was (rollback, the line `— declined`); no answer — the move waits, the next maintenance continues from item 4.
5. **The old scripts** `.claude/hooks/{freshness_check,consent_nudge,judge,raw_guard}.py` — remove (`git rm` or `rm`) those equal to the canon's version (step 3.3) or decided on by the human, and only if no command in any settings file points to them any more; one that stays — name it in the report. The deletion didn't go through — the "Deletion when the environment blocks it" section of the new `lint.md`.
6. **The switch** — per step 3.5: move `.claude/knowledge-check.off` to `.knowledge-check.off` or `tmp/knowledge-check/claude/off`; if the old `judge.py` stayed (item 5) — copy, don't move: the old one reads the old path.
7. **Links to `CLAUDE.md`** in the human's own files (their own roles, their own rules in `methodology/`, `.claude/agents|commands|skills|rules`) — don't edit them: a list with the proposed replacement by `AGENTS.md` — into the report. The wiki is history: don't rewrite it, name broken links.
8. **Last — the main file** (with no moment when `CLAUDE.md` is missing):
   1. write the merge result of step 3.2 as a new file `AGENTS.md` (line 3 — the old one); mark "8.8.1";
   2. rewrite `CLAUDE.md` in place with exactly the line `@AGENTS.md` and a newline; under git — `git add AGENTS.md CLAUDE.md` (git recognizes the rename by content); mark "8.8.2".

   When continuing: `AGENTS.md` exists and `CLAUDE.md` is still the old one — item 8.2; `CLAUDE.md` is already one line — the item is done.

## Step 7. Gate

1. **Under git** — the "after" status with the same command as "before" (step 5.3); the difference is only the paths of step 6 (including the removed scripts and the switch), `wiki/log.md` and the allowances: `.DS_Store`, `.claude/settings.local.json`, `tmp/**`.
2. **Without git** — the inventory outside the step 6 paths = `archive/0007/inventory.txt` (except `archive/0007/`, `archive/index.md`, `wiki/log.md` and the same allowances).
3. `wiki/log.md` — the old content = the beginning of the new; `wiki/index.md` — unchanged.
4. "About the project" in `AGENTS.md` — verbatim as it was in `CLAUDE.md`; `CLAUDE.md` — exactly `@AGENTS.md`.
5. `grep -nE '^(<<<<<<<|>>>>>>>)' <written files>` — empty.
6. **The checks work:** every check command in `.claude/settings.json`, `.codex/hooks.json`, `.qwen/settings.json` points to an existing `hooks/` file; each script answers with one JSON object or stays silent: `echo '{}' | python3 hooks/start.py --program claude` prints one line of JSON; `python3 hooks/env_check.py --program claude` — a "Program: …" line. On Windows the checks don't start yet (their commands are for Mac and Linux): there — only that the commands point to existing `hooks/` files.

**A difference outside the move's paths** (items 1–2) — show it to the human and ask "continue or roll back?". **Items 3–6 broken** — show what didn't match and ask "fix it or roll back?".

**Rollback** — only the step 6 paths; `wiki/log.md` and `wiki/index.md` are not restored. Before it — a copy of the current versions of those paths (under git — `tmp/0007-otkat/`, without git — `archive/0007-otkat.tar`); don't delete it and name it to the human:
- under git — paths that existed at the start: `git restore --source=<start> --staged --worktree -- <paths>` (including `CLAUDE.md` and the removed scripts); paths that didn't exist at the start (`AGENTS.md`, `hooks/`, new files) — delete them; untracked and ignored ones (old scripts, the switch) — restore from `tmp/0007/before/`, then delete `tmp/0007/` (keep the rollback copy `tmp/0007-otkat/`);
- without git — extract only those paths from the archive (`tar xf archive/0007/before.tar <paths>`), delete the created paths, delete the `archive/0007/` folder.

Entries in the personal `~/.claude/settings.json` written at the human's request — put the old ones back and say so. Then the line `move to 3.0.0 — rolled back (<what didn't match>)`, delete the progress file. This is not a decline: at the next maintenance the move can be offered again.

## Step 8. Completion — strictly in this order

1. The mark "8.1" in the progress file, then the journal line `move to 3.0.0 — done`.
2. Line 3 of `AGENTS.md` → `> **Version:** 3.0.0`.
3. Under git — a commit: from the step 6 paths and `wiki/log.md`, present on disk or removed, exclude the ignored ones (`git check-ignore`), then `git add -A -- <paths>` and `git commit -m "update: move to 3.0.0" -- <paths>`; push — per the project's rule.
4. Delete the progress file and the temporary material outside the project (the clone, the analysis). Under git — also `tmp/0007/`. Without git `archive/0007/` stays: it is the human's safety net.

## Step 9. Report

Don't carry out journal entries newer than 3.0.0 in this pass: the move is one maintenance; the rest — at the next one, in a new chat, by the new `lint.md`.

In plain words, without the forbidden words:
- what changed and what of the project's own was carried where;
- **the new rules take effect from a new chat — start the next task in a new one**; a check: in a new chat ask "which version is the project?" — I'll name 3.0.0 without opening files;
- **Claude Desktop** (if the project is open there): in **Project settings → Memory → Project instructions** replace the line with "Before your first reply in every chat, read AGENTS.md from the project folder and follow it; if you can't open the file, say so. Write to files in this folder, not to the app's memory (MEMORY.md).";
- **Codex** (if you work there): trust the folder ("Trust and continue"), type `/hooks` and approve what belongs to this project (this turns on the reminders, the saving prompt and the protection of the source documents), restart Codex;
- **Qwen Code** (if you work there): open it in the project folder interactively and agree when it asks (or type `/hooks` and approve what belongs to this project) — this turns on the reminders, the saving prompt and the protection of the source documents;
- hooks turned off in step 1.6 (`disableAllHooks`) — remove that line from `.claude/settings.local.json`;
- replacements in personal settings the human writes themselves, and old scripts that stayed — what and where;
- links to `CLAUDE.md` in the project's own files — a list with the proposed replacement (step 6.7);
- a shared repository — the branch name and a request to open the MR; others shouldn't run the move before it is merged;
- how to take the project back: under git — `git revert` of the move commit, without git — `archive/0007/before.tar`, except `wiki/log.md` and `wiki/index.md`.
