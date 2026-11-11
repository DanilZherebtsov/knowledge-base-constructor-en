# Bootstrap — restoring the structure

Missing wiki type folders, `specs/`, `data/`, `scripts/`, or a code folder are normal, not a reason to restore anything: they appear with their first page or on first need.

The project arrives ready-made: `CLAUDE.md`, `HELP.md`, `STATE.md`, `methodology/`, `roles/`, `.claude/` (environment machinery: the `SessionStart` hook that forces the session-start checks, the consent note and the rewrite on the word "simpler", the reply reviewer, the `raw/` guard (`PreToolUse`) and the reply style `output-styles/plain.md`), stubs for `wiki/index.md`, `wiki/log.md`, `input/`, `raw/`, `output/`. This file is only for restoring: it runs when something on this list is missing or a file is broken, or on request ("restore the structure"). **Never** from `query`/`lint` — they only report. The hooks and the reply style are restored by maintenance ([lint.md](lint.md), "The project-memory check isn't working", "The `raw/` guard is not in place", "Reply style not enabled").

## Restore (what is missing, without overwriting what exists)

- `input/.gitkeep` — the drop zone for incoming materials (the human throws any materials here; Claude sorts them into `raw/` on ingest, the human never needs to know the subfolders).
- `raw/.gitkeep` — empty; subfolders are not created in advance (see "What is not created in advance").
- `wiki/index.md` (`# Wiki Index`) and `wiki/log.md` (`# Wiki Log`). Wiki type folders are not created: a type appears with its first page.
- `output/.gitkeep` — **project documents and working results**; empty. Subfolders are not created in advance: a designated folder appears when the human names it ([lint.md](lint.md), "Designated folders in `output/`").
- `STATE.md` — seven sections marked `_empty_` ([state-rules.md](state-rules.md), "Structure"); the line `_Updated: YYYY-MM-DD_` — with today's date.

## What is not created in advance

- **Roles** — none are set up in advance; any role can be created at any moment with "create role <name>" (see [roles.md](roles.md)), and, optionally, a role description plus materials for its work can be dropped into `input/` beforehand — then the role will be created qualified right away.
- **The code folder** — never created empty. It appears when code actually arrives or it is decided to write it from scratch, with a one-line notice to the human ([software-engineering.md](software-engineering.md)). `specs/` — with the first spec, `data/` and `scripts/` — on first need.
- **`BRAND.md`** — never created on its own. It appears only **by the human's consent**: once they have accepted the first visual piece of work, Claude offers, once, to save the approved style for the future. Declining is a normal outcome — the file simply does not exist. If a finished brand identity arrives from a client, the file is created right then, while it is being processed ([ingest.md](ingest.md)): that act is the consent. It is always written **from what was built**, never in advance.
- **`tmp/`** — temporary material: what is written for the next step of the work, not for the human (the list is in the "Architecture" tree of `CLAUDE.md`); not created empty, it appears at the first such write. Under git — add `tmp/` to `.gitignore` at creation time, before the first write inside it.
- **`archive/`** — material from `tmp/` that has aged out and is not worth deleting; not created empty, it appears at the first retirement ([lint.md](lint.md)). Under git — add the line `/archive/` to `.gitignore` (with the leading slash: an `archive/` line would also hide `output/archive/`) at creation time, before the first file is moved in.
- **`output/archive/`** — aged-out material from `output/`; not created empty, it appears with the first move, together with its journal `index.md` ([lint.md](lint.md), "Moving into `output/archive/`").
- **`BACKLOG.md`** — deferred items; created with the first such item ([state-rules.md](state-rules.md)).
- **`raw/` subfolders** — never created in advance: an empty folder for a source that does not exist yet is guesswork. The first material is filed on ingest, and that is when the subfolder appears and gets written into the "Architecture" tree of `CLAUDE.md` ([ingest.md](ingest.md), step 2). The human never needs to know the subfolders — they throw things into `input/`.
- Folders beyond the minimum — created when the need appears (the "Structure grows as needed" rule in [CLAUDE.md](../CLAUDE.md)); in `output/` only a folder the human named becomes a designated folder.

After restoring — if the project is under git (the "git is optional" rule in `CLAUDE.md`) — commit `bootstrap: restore structure`; otherwise skip.
