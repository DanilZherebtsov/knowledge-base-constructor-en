---
id: 0006
title: "Moving a 1.x project to 2.0.0 — in place, carrying over what is the project's own"
adr: adr-0063
impact: structural             # build fingerprint → version number; constructor files — wholly 2.0.0
applies-to: any                # any 1.x class except research
requires:                      # any 1.x fingerprint (line 3 of CLAUDE.md with base@N)
  base: "any"
  not: [claim-graph, question-lifecycle, research-project]
produces:
  version: 2.0.0
supersedes: [0001, 0002, 0003, 0004, 0005]   # a project with any base@N comes straight here
---

# Migration 0006 — moving a 1.x project to 2.0.0

> **Executed by the agent INSIDE a 1.x project**, during maintenance, by the bridge entry (`## base@51` and the mechanics entries with the same text), or by step 0 of the new `lint.md` "Update check" if the move was interrupted. This is **the only path** from 1.x to 2.0.0: intermediate journal entries and migrations 0001–0005 are not applied, part numbers in the fingerprint are not raised. Read this file from the tip of the mirror's `main` branch (it has the latest fixes); take the 2.0.0 files **at the tag `v2.0.0`**; the source of the 1.x files is the history of the same mirror. Knowledge — ADR-0063.

## What changes and the main invariant

Constructor 2.0.0 is one ready project with one version: line 3 of `CLAUDE.md` is `> **Version:** 2.0.0` instead of the build fingerprint; decisions, specs, work with your own code, design and roles are in every project and are read by the kind of task. The move is **in place**: the constructor files are replaced by their 2.0.0 versions, and whatever they say about the project itself is carried over into the new files.

**Nothing is written into the project before the human's "yes"** (steps 1–4), except two actions of step 1: committing this same run's auto-fixes (1.4) and `disableAllHooks` with the human's consent (1.5); the analysis and drafts live in a temporary folder outside the project.

> **Invariant.** After the "yes" 0006 writes only:
> - the paths of the `v2.0.0` tree (the closed list, step 2);
> - `BACKLOG.md` — if laying out `STATE.md` requires it;
> - the move lines in `wiki/log.md` (literals below);
> - the progress file;
> - a commit of what this same maintenance run left uncommitted (step 1.4).
>
> Everything else — byte for byte: `wiki/` bodies, `wiki/index.md`, `raw/`, `input/`, `output/`, `specs/`, code, `tmp/`, `archive/` (under git), your own hooks and keys in `.claude/`, your own files named like the mirror's service files (`README.md`, `migrations/`, `CHANGELOG.md`). Temporary things (the mirror clone, the analysis) — **only outside the project**: a foreign `CLAUDE.md` read inside the project will be applied by the environment as an instruction.

**Journal literals** — lines `## [YYYY-MM-DD] update | …` in `wiki/log.md`, matched by the exact text after `| `:
- `move to 2.0.0 — started at <SHA>` (without git — `started without git`) — written after the "yes";
- `move to 2.0.0 — done`;
- `move to 2.0.0 — rolled back (<why>)` — a technical rollback, not the human declining: no pause;
- `move to 2.0.0 — declined`;
- `move to 2.0.0 — declined for good`;
- `can't`.

**Progress file** — created after the "yes": under git — `<git-dir>/0006/hod.md`, where `<git-dir>` = `git rev-parse --path-format=absolute --git-dir` (in a worktree `.git` is a file, the path comes from the command); without git — `archive/0006/hod.md`. In it: "yes received <date>", the start SHA, the closed list, the "what is yours" list with the human's decisions, the list of paths that did not exist at the start, a mark after every file written.

**The move is finished** only when line 3 = `2.0.0`, the journal has `move to 2.0.0 — done`, and (under git) the move is committed.

**Before any question to the human in steps 1–2** — one sentence of the gist: "the constructor has come out in a new form, I can move this project onto it; first one clarification: …".

## Step 1. Entry (in order, without writing)

1. **Journal and progress.**
   - There is `— done` and line 3 = `2.0.0`: under git the move is not committed — run steps 8.3–8.4, otherwise step 8.4; stop, the move is done.
   - There is `— done`, line 3 is a 1.x fingerprint, and the progress file marks item 8.1 — a break in step 8: run step 8 from item 2. No progress file with that mark — the rules have gone back to the old ones (e.g., by merging a branch): tell the human "the project's rules have gone back to the previous ones — looks like a merge; move again?" and wait.
   - There is `— started` with no later `— done` / `— rolled back…` / `— declined…`, and a progress file with "yes received" — ask "the move broke off on <date>, shall I continue?" and on the answer **continue**; under git — if the start (the SHA from the `— started` line) is an ancestor of `HEAD` (`git merge-base --is-ancestor <start> HEAD`) and the commits after it touched only closed-list paths, `BACKLOG.md` and `wiki/log.md` (otherwise — show the human what other paths changed and ask). Continue from the first unmarked file of step 6, the clone and the analysis — anew (steps 2–3); old versions — from `git show <start>:<path>` (without git — from the step 5 backup). An unmarked file differs from the old version — work went on in the project after the break: treat the current one as old and tell the human. The progress file has no main-file size (step 5.5) — repeat step 5 first. No progress file, or no "yes received" in it — start from step 2.
   - Otherwise — start from step 2.
2. **Git, shell, mirror.**
   - No git (`git rev-parse` fails) — the "without git" path (steps 5 and 7).
   - No shell or no access to the mirror (a Claude Desktop project, not the Code tab; a sandbox without network) — the gist sentence and the request: "open the project in the Code tab or in Claude Code — the move will go through there". Write the `can't` line only if the human answered that they cannot or will not; in such an environment do not offer the move for 4 weeks, in an environment with a shell — offer as usual.
3. **Repository** (`git rev-parse --path-format=absolute --show-toplevel --git-dir --git-common-dir`):
   - `--show-toplevel` ≠ the project folder — the project sits inside a larger repository: say so and ask what to do;
   - **a shared repository** — more than one human author in `git log --format=%an` (bots, `noreply` and one person under different addresses do not count; treat a shallow clone as shared; in doubt — ask): the move goes in a separate branch for an MR from the branch where rule changes are merged in this project (if a project rule names it — take it from there; if not — ask). The current branch isn't it — name it and wait: the analysis runs only on that branch. The move is allowed in a git worktree too (`--git-dir` ≠ `--git-common-dir`) if it is on that branch; do not switch branches in someone else's folder;
   - **a single author** — a worktree (`--git-dir` ≠ `--git-common-dir`): do not move, name the main folder. The main branch is `origin/HEAD`; if there is none, but the local branch is the only one and tracks `origin/<same>`, or there is no `origin` at all — it is the main one; otherwise, if the current branch is not the main one — ask;
   - there is an `origin` — `git fetch`. On the branch where rules are merged (a single author — the main one) line 3 in `origin` is already `2.0.0` — the move is done: say "merge <branch> into yours", stop. The current branch is behind it — say so and wait.
4. **Uncommitted changes.**
   - A merge, rebase or cherry-pick is in progress — say so and wait.
   - Made by **this same** maintenance run (auto-fixes) — commit by the project's rule, otherwise with the message `lint: auto-fixes before the move`.
   - Uncommitted edits by the human or the team in closed-list paths, `STATE.md`, `BACKLOG.md`, `wiki/log.md` — do not commit yourself: say so and wait. In other paths — not an obstacle.
   - Untracked and ignored files are not an obstacle; if the path is on the closed list — its copy goes into the backup (step 5).
5. **Foreign hooks and "commit after each reply" rules.**
   - Hooks in `.claude/settings.json`, `.claude/settings.local.json` and `~/.claude/settings.json` that 2.0.0 does not have (2.0.0's own are `freshness_check.py`, `consent_nudge.py`, `judge.py`) that commit or push — offer `"disableAllHooks": true` in `.claude/settings.local.json` for the duration of the move and **continue in a new chat** (hooks are taken at session start). They don't agree — don't start the move. Guards that write nothing (checks before a command) — leave alone.
   - A project rule "commit or push after every reply / piece of work" — do not apply it until step 8; tell the human in one line.

## Step 2. Snapshot (outside the project)

1. **Mirror clone** — at the same address as in step 2 of the "Update check" in your `methodology/lint.md`; outside the project, full (no `--depth`: the 1.x history is needed):
   ```bash
   D=$(mktemp -d) && git clone -q <mirror address> "$D" && git -C "$D" checkout -q v2.0.0
   ```
   No tag `v2.0.0` — to the human: "the new constructor release is not fully out yet, I'll come back to this at the next maintenance"; do not write `can't`, stop.
2. **The closed list** — the files of the `v2.0.0` tree that end up in the downloaded archive:
   ```bash
   git -C "$D" -c core.quotePath=false ls-tree -r --name-only v2.0.0 \
     | while IFS= read -r p; do git -C "$D" check-attr export-ignore -- "$p" | grep -q ': set$' || printf '%s\n' "$p"; done
   ```
3. **The 1.x source — by parts** (line 3 of your `CLAUDE.md`: `base@N · <mechanic>@M · … · <class>@K`):
   - `base/` files (including `ASSEMBLY.md` and the mirror root of that time) — the **last** (newest) mirror commit whose `versions.json` has `base` = N;
   - mechanic files (`mechanics/<name>/`, its `_about.md`) — the last commit where that mechanic's number = M; the class file (`presets/<class>.md`) — where the class number = K;
   - a number that occurs in no commit — the nearest lower one, to the human as a line on the screen; a number ≥ the bridge value (51 for `base`) — the source is the tag `v1-final`.
   ```bash
   # newest to oldest; the first match is the last commit with that number
   for c in $(git -C "$D" log --format=%H v1-final); do echo "$c"; git -C "$D" show "$c:versions.json" 2>/dev/null; done
   ```

## Step 3. What is yours (analysis outside the project)

The agent itself, **by meaning**, for every closed-list file present in the project (skip the stubs under `wiki/`, `raw/`, `input/`, `output/`: step 6 doesn't write them). Compared with **any** 1.x version of the same part in the mirror history (the project may have been updated by journal entries and kept older or newer wording) and with the `v2.0.0` text. The basis for build fill-ins is `ASSEMBLY.md` (from the `base` source commit) and the mechanics' `_about.md` (from their commits): they describe what the build wrote into the slots.

Every place where the project file differs goes into one of three groups:
- **(a) The constructor's** — text of any 1.x version of this part, the `v2.0.0` text, or a slot filled with generic words per the build description (stock class wording, lists of mechanics). Not carried over.
- **(b) About the project itself** — carried into the matching place of the 2.0.0 file:
  - the "About the project" section — verbatim, in place of the 2.0.0 first-contact inset;
  - lines of the "Architecture" tree about your own `raw/` subfolders, your own wiki types, designated folders in `output/`, your own roles and folders;
  - slots filled with facts about the project: currency, units, your own wiki types and the cascades into them;
  - the format and checks of your own wiki type (in `methodology/page-conventions.md`, `lint.md`), the `type` list — merged with the 2.0.0 list, in its form;
  - the human's own rules, including ones that **replaced** a constructor rule — by replacing that same rule in 2.0.0, not by adding next to it;
  - sections marked as an extension — whole.
- **(c) Unclear — onto the screen under "I'm asking".** Also here: a constructor rule the human **deleted** in their copy: 2.0.0 has it, whether to delete it again is their call.

**Separate files:**
- **`HELP.md`** — only what is said about the project itself is carried over; capability lists come from 2.0.0.
- **Role samples, hooks, `.claude/output-styles/plain.md`:** a file equal to that file in **any** mirror commit with the same part number — replace with 2.0.0; otherwise — by meaning, as above. Your own roles (not samples) — don't touch; 2.0.0 samples that are missing — add.
- **`.claude/settings.json`** — merged by entries:
  - your own keys and entries (hooks, `permissions`, `env`) — byte for byte;
  - an entry with a 2.0.0 hook command in its old form (an old `matcher`) — replace it with the 2.0.0 entry, don't add a second one;
  - a 2.0.0 hook entry or `outputStyle` the project did not have — **onto the screen under "I'm asking"** (unless there is an explicit decision against it — then don't add): the journal lines `setup | hooks: not adding`, `setup | style: not adding`, the file `.claude/knowledge-check.off`, an entry the human deleted themselves (visible in `git log -p -- .claude/settings.json`);
  - whatever already stands in `.claude/settings.local.json` — don't duplicate;
  - a shared repository — `outputStyle` only by the question, as in migration 0005.
- **`STATE.md`** — laid out into the seven 2.0.0 sections (Stage · Path to goal · In progress now · Next (1–2 weeks) · Commitments calendar · Blockers, risks, and decisions waiting on me · Recently completed) by a table: every section name from `state-rules.md` of the 1.x source (`base` and the mechanics) → a 2.0.0 section. The main correspondences:
  - "Snapshot" → "Stage";
  - "Open tracks" → "Path to goal";
  - "This week" → "In progress now";
  - "Completed last week", "Answered last week" → "Recently completed";
  - "Blockers and risks" → "Blockers, risks, and decisions waiting on me";
  - "Known tech debt" → `BACKLOG.md` (the file exists — into its matching section, it doesn't — create it);
  - no such section — `_empty_`; `_Updated:_` — the date of the move, the former note after the date — after the closing `_`.
  Not a single content line is lost.

The result of the analysis — in a temporary folder outside the project (`$D-analysis.md`).

## Step 4. One screen, one "yes"

To the human — **one** message, in plain words, without the words "version, tag, merge, migration, fingerprint, mechanic, base" (and the other forbidden words of your `lint.md`). Three groups:
1. **What changes** — 2–3 sentences of the gist (the project moves to a ready set of rules; work with code, decisions, specs and design are there from the start and switch on by task; the help and the memory stay as they were; records are not touched) and **necessarily, by action:**
   - deferred plans will live in a separate file (`BACKLOG.md`) that I don't read at the start of a chat — I open it when a stage closes or "Next" is empty;
   - files in `output/` untouched for a month will be moved to the archive by maintenance (the first time — with a list);
   - the next maintenance will tidy the plan by the new rules.
2. **What is yours and where I carry it** — briefly, group (b).
3. **What I'm asking** — the (c) items; whether to carry your edits into `HELP.md`; whether to lay out the plan (`STATE.md`) into the new sections; whether to switch on what the project did not have: the new reply layout (on top — what to decide now, below — the rest) and checking replies with a separate fast model (a reply sometimes arrives a few seconds later).

And the question: "shall we go ahead? You can also answer 'don't offer again'". Answers to the items — in the same message.
- **"Yes"** — step 5.
- **A partial "no"** to `HELP.md`, `STATE.md`, the reply layout or the check — that file or entry stays as it was, as a journal line after the move (`setup | on the move: HELP.md kept` / `STATE.md kept`; `setup | style: not adding`; `setup | hooks: not adding`).
- **"No"** — the line `move to 2.0.0 — declined`, stop (nothing else has been written into the project). **"Don't offer again"** — `move to 2.0.0 — declined for good`. In both cases delete the temporary material outside the project; if hooks were switched off at step 1.5 — remind the human to remove `disableAllHooks`. A shared repository: write the decline line only if the person answering decides the project's rules (ask); otherwise don't write it, the offer comes back.

## Step 5. Start (after the "yes")

0. **A shared repository** (step 1.3) — create the MR branch from `HEAD` now, before writing; the start = its tip.
1. The journal line `move to 2.0.0 — started at <HEAD SHA>` (without git — `started without git`).
2. The progress file: "yes received <date>", the start SHA, the closed list, the "what is yours" list with the human's decisions on the screen, the closed-list paths and `BACKLOG.md` **that do not exist at the start** (`git cat-file -e <start>:<path>` fails).
3. **Under git — the "before" status** into the progress file:
   ```bash
   git -c core.quotePath=false status --porcelain -uall --ignored=matching
   ```
   Untracked and ignored closed-list paths present on disk — copied into `<git-dir>/0006/before/`.
4. **Without git:** the closed-list paths and `BACKLOG.md` present in the project — as the archive `archive/0006/before.tar`; an inventory outside the closed list (path, size, mtime) — into `archive/0006/inventory.txt`; if there is `archive/index.md` — a line about the `0006/` folder.
5. **Size of the main file:** `LC_ALL=en_US.UTF-8 wc -m CLAUDE.md` (no such locale — `C.UTF-8`); with non-ASCII text the character count must differ from the byte count (`wc -c`) — otherwise the locale did not apply. Write it into the progress file.

## Step 6. Writing

Down the closed list, one file at a time; after each — a mark in the progress file.
- **`CLAUDE.md`** — the 2.0.0 text plus what is carried over (b); "About the project" — in place of the first-contact inset; **line 3 — the old one** (it changes last, step 8).
- **Other files** — 2.0.0 plus what is carried over; `BACKLOG.md` — per the `STATE.md` layout.
- **Paths under `wiki/`, `raw/`, `input/`, `output/`** (2.0.0 stubs) — only if the project has no such folder.
- **Writing hooks** (`.claude/settings.json`, `.claude/hooks/`) failed (environment protection, automatic mode) — the "Writing hooks when automatic mode blocks it" section of the new `methodology/lint.md`. The move goes on regardless; the line `setup | hooks: blocked by the environment` — only if the write never got through.

## Step 7. Gate

1. **Under git** — the "after" status with the same command as "before" (step 5.3); the difference may only be:
   - the paths written in step 6;
   - `BACKLOG.md`, `wiki/log.md`;
   - tolerances: `.DS_Store`, `.claude/settings.local.json`, `tmp/**`.
2. **Without git** — the inventory outside the step 6 paths = `archive/0006/inventory.txt` (except `archive/0006/`, `archive/index.md` and the same tolerances).
3. `wiki/log.md` — the old content = the beginning of the new; `wiki/index.md` — unchanged.
4. Every non-empty line of the old `STATE.md`, except the header before the first `## `, section headings and the `_Updated:_` line (its note is kept), is in the new `STATE.md` or `BACKLOG.md`.
5. "About the project" — verbatim as it was; no first-contact inset in `CLAUDE.md`.
6. `grep -nE '^(<<<<<<<|>>>>>>>)' <files written>` — empty (only the files written: in `raw/` such lines can be material).

**A difference outside the move's paths** (items 1–2) — show the human which other files changed in the meantime (a new cache folder, a program's report, a file they put there themselves) and ask "carry on or roll back?". **Items 3–6 broken** — show what did not add up and ask "fix it or roll back?".

**Rollback** — only the paths written at step 6; `wiki/log.md` and `wiki/index.md` are not restored. Before it, a copy of the current versions of these paths: under git — in `<git-dir>/0006-otkat/`, without git — in `archive/0006-otkat.tar`; don't delete it, name it to the human:
- under git — paths that existed at the start: `git restore --source=<start> --staged --worktree -- <paths>` (only paths for which `git cat-file -e <start>:<path>` succeeds); paths that did not exist at the start — delete, and if their copy is in `<git-dir>/0006/before/` — bring it back from there;
- without git — extract only these paths from the archive (`tar xf archive/0006/before.tar <paths>`), delete the created paths, delete the `archive/0006/` folder.

Then the line `move to 2.0.0 — rolled back (<what did not add up>)`, delete the progress file; if hooks were switched off at step 1.5 — remind the human to remove `disableAllHooks`. This is not a decline: the move can be offered again at the next maintenance.

## Step 8. Finishing — strictly in this order

1. The journal line `move to 2.0.0 — done`; the mark "8.1" in the progress file.
2. Line 3 of `CLAUDE.md` → `> **Version:** 2.0.0`.
3. Under git — the commit: from the step 6 paths, `BACKLOG.md`, `wiki/log.md` that exist on disk drop the ignored ones (`git check-ignore`), then `git add -- <paths>` and `git commit -m "update: move to 2.0.0" -- <paths>`; push — by the project's rule.
4. Delete the progress file and the temporary things outside the project (the clone, the analysis). Under git — also `<git-dir>/0006/`. Without git `archive/0006/` stays: it is the human's safety net.

A break between 1 and 2 — the next maintenance (already by the 2.0.0 `lint.md`, step 0) will see a 1.x fingerprint on line 3 and a progress file marked 8.1, come back here, and step 1.1 will finish line 3.

## Step 9. Report

In plain words, without the forbidden words:
- what changed and what of yours was carried over where;
- the main file's size before and after; over 40,000 characters — say the number (Claude Code warns about a large file);
- **the new rules switch on with a new chat — start the next task in a new one**;
- hooks switched off in step 1.5 (`disableAllHooks`) — remove that line from `.claude/settings.local.json` so they work again;
- the hooks were not written (step 6) — what the human should do;
- a shared repository — the branch name and a request to open an MR; others must not run the move until it is merged;
- how to take the project back: under git — `git revert` of the move commit, without git — `archive/0006/before.tar`, except `wiki/log.md` and `wiki/index.md`.
