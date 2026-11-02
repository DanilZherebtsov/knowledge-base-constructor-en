# `index.md` and `log.md` format

## index.md

Grouped by type (sections — the types that have pages: decisions, discovery, synthesis, principles, entities, architecture), alphabetical within a type. Each line:

```
- [Page title](<type>/<slug>.md) — one-line summary. _Updated: YYYY-MM-DD_
```

The link is **relative to `index.md` itself** (it lives in `wiki/`): no `wiki/` prefix needed. Same for links between pages: the path is relative to the current page (from `wiki/decisions/foo.md` to `wiki/discovery/bar.md` it is `../discovery/bar.md`).

## log.md

Append-only, new entries at the bottom. The prefix `## [YYYY-MM-DD] <op> | <subject>` — for grep (`grep "^## \[" wiki/log.md | tail -20`).

The entry date is the environment's date at the moment of writing, not the file's mtime and not the date the work itself was done. Append-only implies an invariant: each new entry's date ≥ the previous entry's date. A fresh entry with an earlier date almost always means the date was taken from mtime — a violation that lint catches.

Operations in the log:
- `ingest | <source>` — after every ingest, with the list of cascade-updated pages.
- `lint | found N issues, fixed M` — after every lint (except the "already ran today" reply).
- `start | About the project` — once, when the first contact has written the "About the project" section into `CLAUDE.md`.
- `folders | …`, `move | …`, `state | leave it: …`, `numbers | …` — the human's answers to maintenance's questions and its own marks ([lint.md](lint.md)); from them it decides whether to ask again.
- `update | …`, `setup | …` — constructor updates and project settings ([lint.md](lint.md)).
- `decision-closed | <choice>` — a decision was made and extracted into an ADR ([decision-lifecycle.md](decision-lifecycle.md)); closing a spec writes no entry of its own.
