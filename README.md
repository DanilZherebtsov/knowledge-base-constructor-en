# knowledge-base-constructor-en

**The idea — an LLM wiki.** A project's sources are compiled once into a permanent, interlinked wiki; from then on Claude answers by reading it instead of re-deriving the same things from raw documents on every question.

A ready-made project for working with Claude: rules, the project memory (`wiki/`), a work plan, and the order of work with decisions, code, presentations, and roles — all already in place, nothing to attach.

- **How to download:** the **Code → Download ZIP** button on the [public mirror's](https://github.com/DanilZherebtsov/knowledge-base-constructor-en) page — it unpacks into a folder `knowledge-base-constructor-en-main`: that is the project, rename it and open it. Don't move files out of it: some are hidden (`.claude/`). Don't clone it (`git clone`): the archive leaves out the mirror's service files (`README.md`, `CHANGELOG.md`, `versions.json`, `migrations/`), while a clone pulls them in and links the folder to this repository.
- **A project on version 1.x** has nothing to do: maintenance will offer the move. The 1.x files (`base/`, `mechanics/`, `START-HERE.md`) — at the `v1-final` tag.
- **Open:** the folder in Claude Code or Claude Desktop and just start a conversation (in a Claude Desktop project — do the step below first): Claude asks what we'll be working on and records the answer in the project description.
- **Claude Desktop only:** create a project and open its settings (**Project settings**): **Environment → Pre-approved folders → Add** — pick the project folder (the one with `CLAUDE.md` in it); **Memory → Project instructions** — paste the line "Before your first reply in every chat, read CLAUDE.md from the project folder and follow it; if you can't open the file, say so. Write to files in this folder, not to the app's memory (MEMORY.md)." In VS Code, in Claude Code and in the Code tab this step isn't needed.
- **What next:** type `help` at any moment — Claude shows what it can do in this project. The automatic checks need `python3`. Once a week — "run maintenance": a check of the project memory and an offer to update if a new version is out.
- **Versions:** one for the whole project, [`versions.json`](versions.json); what changed — [`CHANGELOG.md`](CHANGELOG.md). The project carries its version number on line 3 of `CLAUDE.md`; an update keeps your edits.
