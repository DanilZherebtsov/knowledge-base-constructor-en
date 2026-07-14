# Bootstrap — initialization for a specific project

Runs at the first ingest into a project with no structure, or on request ("initialize the structure", "set up the wiki"). **Never** from `query`/`lint`. Runs once; again — only to recover the structure.

Bootstrap is not stamping out fixed folders but a **short setup pass tailored to the project**. The template already ships with `CLAUDE.md`, `STATE.md`, `methodology/`, `roles/`; bootstrap builds out the working layers.

## Step 1 — a short interview (3–5 questions; wait for the answers)

<<SLOT INTERVIEW-Q: the class's interview questions (default below)>>

- What is the project and what is the current goal?
- What sources will be coming in?
- Domain conventions (units, citation format — slot S8), if any.

**Always (any class, outside the slot above) — ask about currency:** "What currency does the project's money use, and in what format should amounts be written?" **There is no default** — do not assume a currency from language or country; the answer is recorded in slot S8 "Documents and naming" of `CLAUDE.md` (format examples — with a thousands separator: `$1,200`, `€990`, `220 000 ₽`). A project that uses no monetary amounts — record exactly that ("amounts are not used"). Skip the question if S8 is already filled.

**Always (any class, outside the slot above) — ask about code:** "Will the project have code of its own — a website, landing page, bot, application, scripts?"

- **Yes** → wire up code work (the `software-engineering` mechanic: a code folder at the root, the execution cycle, ownership discipline, and for a web product — the "edits / deploys" role pair).
- **No** → wire up nothing; the capability can be **created later at any moment** — with the phrase "we'll be writing code" or automatically, when code first arrives (the gatekeeper in [ingest.md](ingest.md) will offer it on its own).
- Skip the question if the `software-engineering` mechanic is already attached.

## Step 2 — propose a structure

Based on the answers, Claude **proposes** a layout (never creates it silently): <<SLOT RAW-DEFAULTS: the class's standard `raw/` subfolders; default — one per named source + `misc/`>>; special folders if any came up. The human confirms/edits.

## Step 3 — create (whatever is missing, without overwriting what exists)

- `input/.gitkeep` — the drop zone for incoming materials (the human throws any materials here; Claude sorts them into `raw/` on ingest, the human never needs to know the subfolders).
- `raw/.gitkeep` + the agreed subfolders.
- `wiki/` + the class's type folders (<<SLOT S2>>, each with `.gitkeep`); `wiki/index.md` (`# Wiki Index`); `wiki/log.md` (`# Wiki Log` + `## [YYYY-MM-DD] bootstrap | initialization`).
- `output/.gitkeep` — **the root for working files, present in every class**; created empty, subfolders (`drafts/`, folders for living documents) — as the need arises, not in advance. <<SLOT S4: extra layers for classes with code — `specs/` + `src/` + `data/`>>

## Step 4 — fill in the companion files

- "About the project" in `CLAUDE.md` from the interview answers. After filling it in, **delete the instructional callout** — the `>` block under the "About the project" heading (it is for the template author, not for the project).
- **The "Architecture" tree in `CLAUDE.md`** — write in the `raw/` subfolders that were created (the template does not list them).
- The opening section of `STATE.md` (snapshot/stage); `_Updated:_` = today.

## What bootstrap does NOT create

- **Roles** — none are set up in advance; tell the human that any role can be created at any moment with "create role <name>" (see [roles.md](roles.md)), and that, optionally, they can drop a role description plus materials for its work into `input/` beforehand — then the role will be created qualified right away.
- **The code folder** — never created empty. If code work was wired up (the code question in Step 1) — the folder appears when code actually arrives or gets created, by the human's decision. If it wasn't — the capability can be added later at any moment.
- Folders beyond the minimum, **including working-layer subfolders** (`drafts/`), — created when the need appears (the "Structure grows as needed" rule in [CLAUDE.md](../CLAUDE.md)).

After bootstrap — if under git — commit `bootstrap: initialize structure`; without git, skip.
