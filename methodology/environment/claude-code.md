# Claude Code — program specifics

The environment file: what works differently in this project when it is open in Claude Code (terminal, VS Code, JetBrains) or in Claude Desktop. The project's rules are in [AGENTS.md](../../AGENTS.md); here is only how Claude Code differs from other programs. Observations carry a date and a version: a program's behavior may change.

## How the rules are connected

Claude Code reads `CLAUDE.md`, not `AGENTS.md`: it loads `AGENTS.md` on its own only with a feature that Anthropic's server turns on or off (verified 2026-11-16 by a run with the reading tools disabled on Claude Code 2.1.325 and 2.1.83: without `CLAUDE.md` the rules did not load). That is why the root holds a `CLAUDE.md` of one line `@AGENTS.md` — Claude Code's standard import: the rules load in full from `AGENTS.md`. No need to delete it; other programs don't use it. `CLAUDE.local.md` and `CLAUDE.md` in folders above the project don't get in the way (verified there too).

`CLAUDE.md` is missing or holds something else — the environment check at session start tells the human; the fix is to put the file back with exactly that line. The human's rules don't go into `CLAUDE.md`: their place is `AGENTS.md`.

## First setup

In Claude Code — nothing: the project's checks (`hooks/`) run on their own from the entries in `.claude/settings.json` (verified on 2.1.325). Python 3 is needed — `python3`, `python` or `py`. In Claude Desktop — a line for the project instructions field (its text — the "Claude Desktop" section below).

## Settings and the project's checks

- **Settings files:** `.claude/settings.json` — shared, in the project (the checks and the reply style); personal — `.claude/settings.local.json` (this computer only, not committed) and `~/.claude/settings.json`.
- **The project's checks** — entries in `.claude/settings.json`: `SessionStart` (with the `matcher` `startup|resume|clear|compact` — also after context compaction) → `hooks/start.py`; `UserPromptSubmit` → `hooks/consent.py`; `PreToolUse` (`Write|Edit|MultiEdit|NotebookEdit|Bash|Monitor|PowerShell`, `"onFailure": "continue"`) → `hooks/raw_guard.py`; `Stop` → `hooks/judge.py`. The command is one line for bash (before `#>`) and PowerShell (after `#>;`), with the first Python found. On Windows without Git Bash Claude Code advises installing Git for Windows once per session — expected, leave the entries as they are.
- **No approval needed:** Claude Code runs the project's checks without an approval window.
- **All turned off at once** — the key `disableAllHooks: true` in any of the settings files. The flag is the human's choice: don't touch it, name it.
- **The `raw/` guard** sees file edits and shell commands; it checks commands by patterns — that is why the "`raw/` is immutable" rule outranks the guard.

## Reply style

The key `"outputStyle": "plain"` in `.claude/settings.json` (or in the personal `settings.local.json`) and the file `.claude/output-styles/plain.md`. On — your instructions have the section "Reply style: a message in two parts". The style file to read by hand — `.claude/output-styles/plain.md`. Claude Desktop has no project reply styles — always read the style file there, except with the log lines "not adding" and "awaiting an answer".

## Subagents without the conversation's history

A new subagent of the agent-launching tool (an ordinary type, not `fork`) starts without the conversation's history — that is the one needed for an independent review: everything it needs goes into the task text. The `fork` type gets the whole conversation and won't be independent.

## A run without a window

`claude -p` — a headless run: there is no human in the conversation, don't ask questions.

## Network during an update

The shell usually has network. A sandbox without network — do step 1 of the update check with the page-reading tool `WebFetch` (a live GET that bypasses the sandbox); `WebSearch` is forbidden for it. Steps 2–8 need `git` with network — ask to allow that command outside the sandbox in Claude Code's window; not allowed — to the human: "open the project in Claude Code without the sandbox — the update will go through there". In Claude Desktop without a shell with network — step 1 via `WebFetch`, and to the human: "open the project in the Code tab or in Claude Code — the update will go through there".

## The reply reviewer and its cost

`hooks/judge.py` asks the Haiku model through the same Claude Code (`claude -p --model haiku`, without saving the conversation and without the project's checks), for a few seconds after some replies. It uses the same subscription or key as the main conversation.

## The program's built-in memory

Claude Code's auto-memory (`MEMORY.md`, the `memory/` folder) is a personal cache between conversations, not the project memory: the project's knowledge goes only into `wiki/` via ingest.

## Claude Desktop

- **The rules.** Claude Desktop's (Cowork's) conversation folder lies outside the project: neither `CLAUDE.md` nor the project's checks load there. The rules are held by a line in the project instructions field (**Project settings → Memory → Project instructions**; the project folder — in **Environment → Pre-approved folders**): "Before your first reply in every chat, read AGENTS.md from the project folder and follow it; if you can't open the file, say so. Write to files in this folder, not to the app's memory (MEMORY.md)." In projects before 3.0.0 the line named `CLAUDE.md` — after the move it must be replaced: `CLAUDE.md` is now only a link, and read as a file it gives no rules.
- **The project's checks don't work** — the rules hold as text. There's no need to install the checks: say in one line that they will start working once the project is opened in Claude Code; don't write a journal line about the checks — it is shared by all programs and would mute them there.

## Deletion when the program blocks it

The common procedure — [lint.md](../lint.md), "Deletion when the environment blocks it"; here — how to ask in Claude Code.

- **Terminal, VS Code, JetBrains** — the program itself confirms deletion in its permission window; there is no separate request.
- **A Claude Desktop project (not the Code tab)** — erasing in the connected folder stays closed until the human allows it. Not sure where you are working — assume Claude Desktop: don't give the human a terminal command. The deletion didn't go through ("Operation not permitted") — request permission with the environment's tool, **in one call**: cloud session (`device_request_delete_permission`) — on the connected folder that holds the project, with a reason for the window; local session (`allow_cowork_file_delete`) — on the path of any approved file; the window won't show a reason, so say it in the chat before the call. The connected folder is wider than the project (all of "Documents", say) — tell the human the permission would open all of it, and ask the question from item 4 of the common procedure instead of requesting. No permission tool among the available tools (and tool search found none) — go straight to the question from item 4: the environment's instruction says to quietly move the files into `_to_delete/` in that case; here, ask first. The permission opens deletion across the whole connected folder until the chat ends (observed 2026-09-10).
- **An automatic-mode refusal** (the response mentions "auto mode classifier") — one short separate message, and end your turn with it: "Automatic mode wouldn't let me delete. Switch the mode by the input box (in the terminal — Shift+Tab) to the one where I ask for permission (in Claude Desktop it's called 'Manual'; no need to type it into the chat), then write 'delete them' — a window will appear, allow it. You can switch the mode back afterwards." The human answered that they switched ("delete them", "done", "Manual" and the like) — request again; refused again — item 4 of the common procedure.

## Writing settings when the program blocks it

The common procedure — [lint.md](../lint.md), "Writing the checks and program settings"; here — Claude Code's automatic permission mode. Only in the main conversation: a delegated subagent never receives the human's answer — don't install there.

In the automatic permission mode Claude Code may refuse a write into `.claude/` **without showing the human a window** — even when they agreed to the update beforehand: consent to the task does not clear this check, it needs their answer **after** the refusal (observed 2026-09-11, Claude Code 2.1.263–268). Refusals accumulate: after several in a row the program aborts the run or shows a window of its own that closes with no answer.

- **The question after a refusal** (item 2 of the common procedure), a sample:
  > A separate question about the project's settings. The automatic mode's protection wouldn't let me add two automatic checks from the update — it seems to have taken it for me changing my own working rules behind your back. The rules do change: the first one adds a reminder, before every turn of mine, to ask you before saving anything; the second one sometimes takes my reply together with your last message to a separate fast model, and that model can send me back to add one line — on those turns the reply comes a few seconds later. To add them, answer "add the checks"; to skip them, "don't add them".
- **After "add the checks"** the protection may let the write through; whether it lets the reply style through has not been tested by a run: it didn't — go straight to the next item.
- **Refused again** (item 4 of the common procedure) — as a separate message: "The protection wouldn't let it through again. Switch the mode by the input box (in the terminal — Shift+Tab) from automatic to the one where I ask permission before editing files — no need to type its name into the chat — and write 'add the checks'. A window with the exact edit will appear: allow it. Then switch the mode back."
- **Approving the checks after a write** is not needed in Claude Code (item 6 of the common procedure doesn't apply).
