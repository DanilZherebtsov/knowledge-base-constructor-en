# Wiki page conventions — frontmatter, types, format

Read before creating a page, editing frontmatter, or revising the rules.

## Frontmatter

```yaml
---
type: <<SLOT S2: the class's list of types — decision | discovery | synthesis | principles | ...>>
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

**A citation carries a location.** A source link comes with the specific place (page / paragraph / timecode); without it, it's a paraphrase, not a citation. The location format is domain-specific (see slot S8). This is a write-side rule: it governs new pages; no retroactive cleanup of old ones is required.

## Atomicity

One concept per page. Soft limit 400 lines, hard limit 800. Past 400 — split out subpages and link them. Any single read stays within the context window.

## Cross-links

Markdown links with relative paths: `[Title](../type/slug.md)` for wiki, `[Description](../../raw/.../file.md)` for raw sources, `[Title](https://...)` for URLs. Anchor text is mandatory and human-readable.

## Journal pages

A page that accumulates observations: each entry starts with `## [YYYY-MM-DD] Title` — freshness is visible at a glance.

## File-name prefixes

Inside a flat type folder, pages are grouped by **name prefix** — the leading token of the file name (`<prefix>-<slug>.md`). The prefix replaces subfolders (depth = 1) and serves as a filter key: visible in `index.md` links and via `ls <type>/<prefix>-*`. That is how a role/query finds its slice without reading the whole wiki.

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
Decisions are marked `superseded`, never deleted. The flow leading up to an ADR — <<SLOT S6: the class's lifecycle file>>.

### synthesis/ — written-back answers

Comparisons, retrospectives, cross-cutting analyses, briefings. Required: an introduction (what the question was) and links to the sources/pages it is built on. Synthesis gets rewritten (edit the existing page rather than spawning a new one).

### principles/ — rules extracted from work

A rule "always X / never Y" from an incident → a page per applicability (`process.md`, `domain.md`, …). Entry format: `## P-XX — name`, **Rule / Why / When to apply / Precedent / Source**. Without a source a principle is not recorded. Read before a nontrivial task (the "think" phase).

### discovery/ — knowledge about the outside world

A segment, a competitor, a fact, a regulation, an insight. Grouped by prefixes. Hypotheses carry a status: `open`/`validated`/`invalidated`.

<<SLOT S3: the class's central/domain type — describe its format here.
Examples: architecture/ (modules, contracts, mandatory `## Implementation` section + `implementation:` field);
claims/ (a claim + evidence chain + supports/contradicts/refines links);
entities/ (a counterparty card: Status/Who/Contacts/Terms + History + Links).
If the class does not use discovery/ — remove its block above.>>
