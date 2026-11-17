# Codex — program specifics

The environment file: what works differently in this project when it is open in Codex (OpenAI; terminal, VS Code, the app). The project's rules are in [AGENTS.md](../../AGENTS.md); here is only how Codex differs from other programs. Observations carry a date and a version: a program's behavior may change.

## How the rules are connected

Codex reads `AGENTS.md` on its own — from every folder from the git root down to the folder it is opened in (without git — from the launch folder). Codex doesn't read `CLAUDE.md`: that line is for Claude Code, no need to delete it.

- **The limit.** By default Codex reads only the first 32 KiB of the rules files (`project_doc_max_bytes`) and silently drops the rest. This project's `AGENTS.md` is larger, so the project's own setting raises the limit — `.codex/config.toml`; Codex applies it only in a folder that has been trusted. Without trust — the line `project_doc_max_bytes = 262144` at the top of Codex's personal settings file (`~/.codex/config.toml`, before the first line in square brackets) and a restart. The environment check counts the size and tells the human only if the rules don't fit; the sign of truncation for you yourself — the very end of your instructions lacks the last line of `AGENTS.md`, an italic marker about the end of the rules file: don't take on the task.
- **`AGENTS.override.md`** next to `AGENTS.md` is read by Codex instead of it. The environment check tells the human.
- **Opened in a subfolder of a project without git** — the root `AGENTS.md` isn't found: ask the human to open Codex in the project folder.

## First setup

Once per project (verified on Codex 0.169, 2026-11-16):
1. **Trust the folder** — Codex asks on first open: "Trust and continue", not "Open restricted". Without trust neither the project's settings (limit, style) nor its checks work.
2. **Approve the project's checks** — type `/hooks` and approve this project's checks.
3. **Restart Codex** — until then it reads the previous settings.

Python 3 is needed — `python3`, `python` or `py`. A project without git works (verified there too). On Windows in PowerShell 5.1, read project files with `-Encoding UTF8` and edit them with your own editing tool, not PowerShell commands: otherwise non-Latin text gets corrupted.

## Settings and the project's checks

- **Settings files:** `.codex/hooks.json` — the project's checks; `.codex/config.toml` — the limit and the reply style; `.codex/agents/` — subagent roles (the `reviewer`). Personal — `~/.codex/config.toml` (or the folder from `CODEX_HOME`).
- **The project's checks** — entries in `.codex/hooks.json`: `SessionStart` → `hooks/start.py`; `UserPromptSubmit` → `hooks/consent.py`; `PreToolUse` (`Bash|apply_patch`) → `hooks/raw_guard.py`; `Stop` → `hooks/judge.py`. The command (`command` — Mac and Linux, `commandWindows` — Windows, PowerShell) finds the project root by `.codex/hooks.json` and runs the script with the first Python found; a failure doesn't stop the work.
- **Turning all checks off at once** — reject them in `/hooks` (we haven't checked a separate project switch in Codex's settings).
- **Approval is by the entries' contents.** An entry's command in `.codex/hooks.json` for the human's system changed — `command` on Mac and Linux, `commandWindows` on Windows (after a project update or an edit) — the approval is dropped and the checks silently don't run until the human approves them again in `/hooks`. Editing the `hooks/` scripts themselves doesn't drop approval; changed entries are named by the journal under "How to accept".
- **The `raw/` guard** sees edits via `apply_patch` and shell commands; it checks commands by patterns. It doesn't see input into an already running program (`write_stdin`) — verified 2026-11-16 on Codex 0.169: a write into `raw/` from a running interpreter went through; nor the tools of connected servers. That is why the "`raw/` is immutable" rule outranks the guard: don't write into `raw/` by any route. Don't work around the guard's refusal and don't ask to allow it in Codex's window.

## Reply style

The style text is the `developer_instructions` key in `.codex/config.toml` (the same text as in `.claude/output-styles/plain.md` and `.qwen/output-styles/plain.md`). Codex applies it when the folder is trusted — then your instructions have the section "Reply style: a message in two parts". No such section — read that key's text at the start of the chat and after context compaction and follow it. Codex has no "only for you" option: the personal settings file `~/.codex/config.toml` is shared by all of the human's projects — to "for the whole team or only for you" say so and offer "for the whole team" or "don't add it".

## Subagents without the conversation's history

The `spawn_agent` tool — **strictly with `fork_turns="none"`**: by default the child agent gets the whole conversation, and the review looks independent without being so; everything it needs goes into the task text. A ready reviewer is the `reviewer` agent from `.codex/agents/`. A subagent's sandbox inherits the main conversation's permissions (verified 2026-10-26: `sandbox_mode = "read-only"` in the agent file was not applied) — tell the reviewer plainly in the task "don't create or change anything". Codex holds a limited number of agents at once — launch reviewers in waves: the cap is not a failure of the mechanism.

## Commits

Codex's sandbox doesn't allow writing into `.git/` (verified on 0.164): a commit after an ingest or maintenance needs approval in the window — ask; refused — skip the commit and tell the human the changes are not saved in history.

## A run without a window

`codex exec` — a headless run: there is no human in the conversation, don't ask questions.

## Network during an update

Codex's sandbox usually has no network (the command fails with `Could not resolve host`, `hooks/update.py` — with `RESULT failed:network`) — that is not the human's refusal: repeat the same update-check commands (`hooks/update.py` with a timeout of up to 2 minutes), asking the human in Codex's window for network permission — only for these commands, not for good. Not allowed — the check is not done; the action for the human: "allow Codex network access for this check in the approval window, or open the project in Codex with network access — the update will go through there". Codex's built-in web search is forbidden for this check.

## The reply reviewer and its cost

After some replies `hooks/judge.py` starts a separate short `codex exec` (without saving the conversation, without personal settings, read-only, low reasoning effort) — about 10 seconds (measured 2026-11-16: 8.7 s). When Codex is signed in with an API key, each such run costs money; with a subscription — it uses its limits. No `codex` command where the checks look for it — the environment check tells the human.

## The program's built-in memory

Codex's memory (`memories = true` in the `[features]` section of the personal settings) is a personal cache that bypasses the project memory and isn't seen in other programs: the project's knowledge goes only into `wiki/` via ingest. The environment check tells the human if the memory is on.

## Deletion when the program blocks it

The common procedure — [lint.md](../lint.md), "Deletion when the environment blocks it"; here — how to ask in Codex. The sandbox keeps erasing and writing outside permitted paths closed until the human approves them in a window (the sandbox doesn't allow writing into `.codex/` and `.git/` — verified on Codex 0.164, 2026-10-26; the approval windows were not tested by a run — the description follows Codex's documentation).

- **The request:** one attempt through the approval window — repeat the same command so that Codex asks the human; the reason — one sentence in the chat before the call.
- **The human's refusal in the window** — item 4 of the common procedure (leave them or move into `_to_delete`), don't ask again.
- **A mode where Codex doesn't ask and forbids** (read-only) — one short separate message: "Codex wouldn't let me delete. Switch the permission mode to one where Codex asks before changes, and write 'delete them'". They answered that they switched — repeat once.

## Writing settings when the program blocks it

The common procedure — [lint.md](../lint.md), "Writing the checks and program settings"; here — Codex.

- The sandbox doesn't allow writing into `.codex/` without approval — that is not the human's refusal: ask them to allow it in Codex's window.
- What is needed from the human in the question (item 2 of the common procedure): allow the write in the window, switch the permission mode, trust the folder, or approve the checks in `/hooks` — name what is needed.
- **After writing `.codex/hooks.json`** — if the command for the human's system changed, ask them to approve the checks again (`/hooks`) and restart Codex (item 6 of the common procedure).
- In the description for the human, name the cost of the after-reply check: "when signed in with an API key, this costs money".
