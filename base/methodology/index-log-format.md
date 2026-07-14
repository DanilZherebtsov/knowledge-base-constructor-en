# `index.md` and `log.md` format

## index.md

Grouped by type (<<SLOT S2: sections = the class's set of types>>), alphabetical within a type. Each line:

```
- [Page title](<type>/<slug>.md) — one-line summary. _Updated: YYYY-MM-DD_
```

The link is **relative to `index.md` itself** (it lives in `wiki/`): no `wiki/` prefix needed. Same for links between pages: the path is relative to the current page (from `wiki/decisions/foo.md` to `wiki/discovery/bar.md` it is `../discovery/bar.md`).

## log.md

Append-only, new entries at the bottom. The prefix `## [YYYY-MM-DD] <op> | <subject>` — for grep (`grep "^## \[" wiki/log.md | tail -20`).

The entry date is the environment's date at the moment of writing, not the file's mtime and not the date the work itself was done. Append-only implies an invariant: each new entry's date ≥ the previous entry's date. A fresh entry with an earlier date almost always means the date was taken from mtime — a violation that lint catches.

Operations in the log:
- `ingest | <source>` — after every ingest, with the list of cascade-updated pages.
- `lint | found N issues, fixed M` — after every lint.
- `bootstrap | initialization` — once, when the structure is first set up.
- <<SLOT CLOSE-OP: the domain operation for closing a unit of work — research: `question-closed | Q-NNN`; business: `decision-closed | <choice>`; saas: none>>.
