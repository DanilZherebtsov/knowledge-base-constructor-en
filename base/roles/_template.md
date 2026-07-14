# Role: <name, e.g. "Lawyer">

> Model file. Copy to `roles/<role>.md` and fill it in for your project. Mechanics — [methodology/roles.md](../methodology/roles.md).

## Area of responsibility

<What the role does. 2–4 bullets.>

## What it does NOT do

<Boundaries; where to redirect (to another role / to the owner).>

## My wiki slice (prefixes and tags)

<How the role finds its pages: e.g. `discovery/<prefix>-*`, tag `<tag>`. Claude filters via `wiki/index.md` + `grep` on `tags:`, reads only the matches.>

## What to read at the start

- `CLAUDE.md` — shared context.
- my wiki slice (above) and the related raw materials.

## Where to write findings

- Facts → `wiki/discovery/` (or the role's home type) via ingest, with a source.
- Draft analysis / a recommendation → the class's working layer.
- An accepted decision → an ADR in `wiki/decisions/` via the class's lifecycle file.

## Tone and format

<How to answer. No inventing — when unsure, mark `[needs verification]`.>
