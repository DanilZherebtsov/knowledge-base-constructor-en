# Qwen Code — program specifics

The environment file: what works differently in this project when it is open in Qwen Code. The project's rules are in [AGENTS.md](../../AGENTS.md); here is only how Qwen Code differs from other programs. Observations carry a date and a version: a program's behavior may change.

## How the rules are connected

Qwen Code reads `AGENTS.md` on its own: it is its default rules file (the `context.fileName` setting). It doesn't read `CLAUDE.md`: that line is for Claude Code, no need to delete it.

- **A personal `context.fileName` setting without `AGENTS.md`** — the project's rules don't load. The environment check at session start tells the human what to add.
- **Folder trust.** Off by default. If the human turned it on in their settings (`security.folderTrust.enabled: true`) and didn't trust this folder, Qwen Code **silently** reads neither the rules, nor the project's settings, nor its checks (verified on 0.26.1, 2026-10-26). From the inside nobody can see this — neither you nor the checks; that is why the mirror's README tells the human about it.
- **Started outside the project folder** — Qwen Code takes the project's checks only from the folder it starts in. The environment check tells the human to open Qwen Code in the project folder.

## First setup

Once per project: open Qwen Code in the project folder interactively (not `qwen -p`) and **approve the project's checks** when it asks about them, or type `/hooks` and approve what belongs to this project. To the human — in the words "turn on the reminders, the saving prompt and the protection of the source documents" ([lint.md](../lint.md), "How to describe the checks to the human"), without the word "checks". Without approval the checks don't load — Qwen Code's output has the line "Project hooks in .qwen/settings.json were not loaded: they are not approved for this folder" (verified on 0.27.4, 2026-11-16). Qwen Code's warning that the always-on context is longer than 10,000 tokens (`Loaded always-on context … more than 10,000 tokens`) is expected for this project: the rules are that size — if the human asks, say so. What exactly the approval window shows — per Qwen Code's code, not tested by a run. `python3` is needed.

## Settings and the project's checks

- **Settings files:** `.qwen/settings.json` — the project's checks and the reply style; `.qwen/output-styles/plain.md` — the style file. Personal — `~/.qwen/settings.json`; approval decisions — `~/.qwen/hookApprovals.json`.
- **The project's checks** — entries in `.qwen/settings.json`: `SessionStart` → `hooks/start.py`; `UserPromptSubmit` → `hooks/consent.py`; `PreToolUse` (`write_file|edit|notebook_edit|run_shell_command|monitor`) → `hooks/raw_guard.py`; `Stop` (timeout 90 s) → `hooks/judge.py`. The command is `python3 hooks/<script>.py --program qwen || py -3 … || python … || exit 0`, the path from the project folder, no double quotes: on Windows it goes to cmd (`ComSpec` pointing at PowerShell 5.1 is not supported).
- **Turning all checks off at once** — reject them in `/hooks` (we haven't checked a separate project switch).
- **Approval is by contents** — of the checks block in `.qwen/settings.json` and of the project's `.env` file. Either changed (a project update, an edit of `.env`) — the approval is dropped and the checks don't load until the human approves them again. Editing the `hooks/` scripts themselves doesn't drop approval; changed entries are named by the journal under "How to accept". Rejected checks Qwen Code doesn't offer again — they can be turned on only in `/hooks`.
- **The `raw/` guard** sees file writes and edits, shell commands and `monitor`; it checks commands by patterns — that is why the "`raw/` is immutable" rule outranks the guard.

## Reply style

The key `"outputStyle": "plain"` in `.qwen/settings.json` and the file `.qwen/output-styles/plain.md` (the same text as Claude Code's). On — your instructions have the section "Reply style: a message in two parts". No such section — read `.qwen/output-styles/plain.md` at the start of the chat and after context compaction. Qwen Code has no "only for you" option: the personal settings file `~/.qwen/settings.json` is shared by all of the human's projects — to "for the whole team or only for you" say so and offer "for the whole team" or "don't add it".

## Subagents without the conversation's history

The `agent` tool with a named subagent type (the built-in general one; not `fork`) starts it without the conversation's history — no type other than `fork` in the list: reviewers can't be started, tell the human; everything it needs goes into the task text; `fork` passes the whole conversation, and it won't be independent (per Qwen Code 0.26.1's code; not tested by a run in this version of the project). Tell the reviewer plainly in the task "don't create or change anything".

## A run without a window

`qwen -p` — a headless run: there is no human in the conversation, don't ask questions. The `-p` output holds only the last reply of the turn (observed 2026-11-16).

## Network during an update

Qwen Code's shell usually has network (the sandbox is off by default). Without network — do step 1 of the update check with the page-reading tool `web_fetch`; the built-in web search is forbidden for it. Steps 2–8 need network — `git` or `hooks/update.py` (`github.com`, `codeload.github.com`, `raw.githubusercontent.com`) — to the human: "start Qwen Code with network access (without the sandbox) — the update will go through there".

## The reply reviewer and its cost

After some replies `hooks/judge.py` starts a separate short `qwen -p` (safe mode, without recording the conversation, one turn) — with the same model the human has configured. The cost is that of one short request to that model; on a local model the answer may come later, up to the 90 s timeout.

## The program's built-in memory

Qwen Code's built-in auto-memory makes a separate request to the model after every reply and may write a note into its own folder `~/.qwen/projects/…/memory/` (observed 2026-11-16, 0.27.4). The `save_memory` tool writes into the human's personal rules file `~/.qwen/QWEN.md`. Neither is the project memory: the project's knowledge goes only into `wiki/` via ingest; don't use `save_memory` for it. Auto-memory is on (the default) — the environment check tells the human once per project, with how to turn it off: `"memory": {"enableManagedAutoMemory": false}` in Qwen Code's settings.

## Deletion when the program blocks it

The common procedure — [lint.md](../lint.md), "Deletion when the environment blocks it". Qwen Code asks the human before commands and edits in its confirmation window; a refusal in the window — item 4 of the common procedure. In a mode where Qwen Code doesn't ask and forbids — tell the human: "Qwen Code wouldn't let me delete. Allow it in the confirmation window, or switch the confirmation mode to one where it asks, and write 'delete them'". Not tested by a run.

## Writing settings when the program blocks it

The common procedure — [lint.md](../lint.md), "Writing the checks and program settings"; here — Qwen Code.

- **After writing `.qwen/settings.json`** the approval is dropped (it is by contents): ask the human to turn on the reminders, the saving prompt and the protection of the source documents again: type `/hooks` and approve what belongs to this project, or open Qwen Code in the project folder interactively and agree (item 6 of the common procedure).
- A write refused in the confirmation window — what is needed from the human (item 2 of the common procedure): allow the write in the window or switch the confirmation mode.
