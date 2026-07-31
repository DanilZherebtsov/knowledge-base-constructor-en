# Ingest — adding new knowledge

Triggered by phrases: "process this document", "remember this", "capture this", "add to the wiki".

When the human throws files into `input/` (the drop zone for incoming materials), points to a source, or puts a file into `raw/`:

1. **Read the source.** If it is long (multi-page material, a long transcript) — read it in parts, don't skim. First discuss the key takeaways and confirm intent before changing the wiki.
2. **Recognize the input and place it — layout is Claude's job, not the human's.** Before placing anything, recognize what it is: **working source code** (a code tree, a deployable site, scripts, a project with a build/manifest) or **regular material** (a document, a transcript, data, someone else's code sample for study)?
   - **Working code** — do NOT bury it in `raw/`. Ask the human in plain words: "is this code you'll maintain and develop here, or someone else's example to study?". If **we own / change / deploy it** → the code lives in a **root folder named for what it is** (`landing/`, `src/`, `scripts/`), created by the human's decision; **not** `raw/` and **not** `output/`. The files on disk are the source of truth; only knowledge about the code goes into the wiki (what it is, key facts — the offer, pricing, operations), never the code itself. <<SLOT OWNED-CODE: with the `software-engineering` mechanic active — replace with: "code-folder layout, code-ownership conventions, the execution cycle, and role suggestions are governed by [software-engineering.md](software-engineering.md)". In plain base without the mechanic — keep: "if the project seriously starts owning code, suggest that the human attach the `software-engineering` mechanic".>> If the human says "it's an example to study" — proceed as with regular material.
   - **Regular material** — move the source from `input/` (or from wherever indicated) into a fitting `raw/` subfolder, `raw/<concern>/YYYY-MM-DD-slug.md`; if none fits — create a new one and add it to the `CLAUDE.md` tree (the "Structure grows as needed" rule). The human never needs to know the `raw/` subfolders — they throw things into `input/`. Preserve the original. **Strip secrets** (passwords, keys, unnecessary personal data). Add a metadata header: source (URL, if any), date received, publication/document date (if known), authors. After placement `input/` is empty (files moved into `raw/`).
3. **Compile into the wiki.** The unit of knowledge is <<SLOT KNOWLEDGE-UNIT: a page (default) | a `claim` for research>>. For each affected unit:
   - **Same thesis** → merge, add the source to `sources:`, update point-wise.
   - **New concept** → a new page in the fitting type folder, named for the concept (not for the file name). For types with prefix grouping — use the prefix. Type choice and format — [page-conventions.md](page-conventions.md).
   - **Contradiction** → annotate the conflict in the text, present both sides, escalate to the human for resolution. Never pick a side silently.
   - **Working as a role** (`roles/<role>.md` is open) → the page gets the role's slice marking: the name prefix on a new page, the role's tag in `tags:` — both on a new page and on the one appended to (no tag → add it). Without the marking the finding drops out of the role's slice for good: marking at role creation is one-off, while writes keep coming. Mechanics — [roles.md](roles.md).
4. **Cascade.** Scan the same type folder and the index for materially affected pages. Update them, refresh `updated:`.
5. **Update `wiki/index.md`** — entries for the affected pages. Format — [index-log-format.md](index-log-format.md).
6. **Append to `wiki/log.md`:**
   ```
   ## [YYYY-MM-DD] ingest | <name of the main page>
   - Updated: <cascade-updated page>
   ```
7. **Commit — if the project is under git.** One commit, message `ingest: <source>`. Without git the step is skipped (history lives in `log.md` and page dates).

An important rule: **when updating any claim — reread the raw source it references.** Do not treat the wiki text as the primary source — that is how drift creeps in.

**A big source** (an hour-plus interview, a detailed report): don't extract everything in one pass — 3–5 key points per ingest, mark the rest right in the raw file ("min. 45:00: about X — to review") and come back later.
