# Wiki page conventions — frontmatter, types, format

Read before creating a page, editing frontmatter, or revising the rules.

## Frontmatter

```yaml
---
type: decision | discovery | synthesis | principles | entities | architecture
status: active | superseded | draft   # + domain values as needed (open/validated/invalidated, inactive…)
updated: YYYY-MM-DD
sources:                  # mandatory
  - ../../raw/<concern>/2026-05-18-slug.md
  - https://...
supersedes: [adr-0007]    # optional
superseded_by: [adr-0014] # optional
tags: [topic1, topic2]
---
```

**`sources:` is mandatory.** A claim without a source is a hallucination dressed up as fact; lint rejects pages without sources.

**A citation carries a location.** A source link comes with the specific place (page / paragraph / timecode); without it, it's a paraphrase, not a citation. The location format depends on the kind of source. This is a write-side rule: it governs new pages; no retroactive cleanup of old ones is required.

## Atomicity

One concept per page. Soft limit 400 lines, hard limit 800. Past 400 — split out subpages and link them. Any single read stays within the context window.

## Cross-links

Markdown links with relative paths: `[Title](../type/slug.md)` for wiki, `[Description](../../raw/.../file.md)` for raw sources, `[Title](https://...)` for URLs. Anchor text is mandatory and human-readable.

## Journal pages

A page that accumulates observations: each entry starts with `## [YYYY-MM-DD] Title` — freshness is visible at a glance.

## File-name prefixes

Inside a flat type folder, pages are grouped by **name prefix** — the leading token of the file name (`<prefix>-<slug>.md`). The prefix replaces subfolders (depth = 1) and serves as a filter key: visible in `index.md` links and via `ls <type>/<prefix>-*`. That is how a role/query finds its slice without reading the whole wiki.

## ADR and principle numbers

A number is the next after the largest, with the same digit count as its neighbours. For ADRs it runs across the whole wiki. For principles — within a series: the series is the letter part of the ID (`P-D52` — series `D`); for IDs with no letters (`P-01`) — as the project has it: across the wiki, or from the start in each file. Look for the largest not only in the branch that is open:

- among the files of the working folder, including those not committed yet;
- under git — in all branches, local and remote. From the project root: `GIT_TERMINAL_PROMPT=0 git fetch --all` (it failed — count by what there is and tell the human), then for each branch from `git for-each-ref --format='%(refname)' refs/heads refs/remotes` — `git -c core.quotePath=false ls-tree --name-only <branch> wiki/decisions/` (ADRs) and `git grep -h -E '^#+ P-' <branch> -- wiki/principles` (principles). There are more branches on the server than you can see (`git ls-remote --heads origin`) — say so: the survey is incomplete.

Before merging your branch into the shared one — check once more: the number has been taken meanwhile — renumber before the merge, not after. Repeats that happen anyway are found by maintenance ([lint.md](lint.md), "Numbers issued twice").

## Page types — format

### decisions/ — ADR (universal)

```
# ADR-0012 — <the decision in one phrase>

Status: active
Date: 2026-05-18
Context: <the problem and constraints>
Decision: <what was decided>
Consequences: <what gets easier, what gets harder>
Alternatives considered: <briefly, with the reason for rejection>
Sources: <citations/links>
```
Decisions are marked `superseded`, never deleted. Number — the "ADR and principle numbers" section above. The flow leading up to an ADR — [decision-lifecycle.md](decision-lifecycle.md); from a spec — [spec-lifecycle.md](spec-lifecycle.md).

### synthesis/ — written-back answers

Comparisons, retrospectives, cross-cutting analyses, briefings. Required: an introduction (what the question was) and links to the sources/pages it is built on. Synthesis gets rewritten (edit the existing page rather than spawning a new one).

### principles/ — rules extracted from work

A rule "always X / never Y" from an incident → a page per applicability (`process.md`, `domain.md`, …). Entry format: `## P-XX — name` (the number — the "ADR and principle numbers" section above), **Rule / Why / When to apply / Precedent / Source**. Without a source a principle is not recorded. Read before a nontrivial task (the "think" phase).

### discovery/ — knowledge about the outside world

A segment, a competitor, a fact, a regulation, an insight. Grouped by prefixes, e.g. `market-` `competitor-` `norm-` `price-`. Hypotheses carry a status: `open`/`validated`/`invalidated`.

### entities/ — counterparties and people

A counterparty card. Grouped by prefixes: `supplier-` `client-` `contractor-` `staff-` `landlord-` `partner-`.

```
# <Counterparty name>

Status: <active | chosen | rejected | archived>
Who: <role in the project>
Contacts: <phone/email/channel>
Terms: <rate, commission, term, etc. — with a source>

## History
<the timeline of dealings>

## Links
<links to ADRs, deals, other cards>
```

### architecture/ — how our own code is built

Modules, services, screens, contracts (APIs, data formats, import/export), invariants, multi-step product scenarios, and the **runtime-data contract** (`data/`: the schema + how it's updated — not the data itself). One page — one module, contract, or scenario; grouped by name prefixes.

The frontmatter additionally carries an `implementation:` field listing the paths to the code that implements the page (no code yet — `implementation: []`). A mandatory section:

```markdown
## Implementation

- `<code-folder>/<path>` — what is implemented here
```

The field is a path check (lint catches a missing field and broken paths), not a detector of semantic divergence: when the code changes, the execution cycle updates the page ([software-engineering.md](software-engineering.md)). The truth about behavior is the code — the "code in the code folder beats the wiki" rule in `CLAUDE.md`.
