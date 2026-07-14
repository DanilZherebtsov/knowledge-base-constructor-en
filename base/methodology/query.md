# Query — a structured answer with citations

Triggered by phrases: "what do we know about X", "make a brief on Y", "compare A and B from the wiki", "justify decision Z", "have we discussed this before?".

1. Read `wiki/index.md` and select candidate pages — filtering by **name prefix** and, if needed, `grep` on the `tags:` frontmatter field. Only the relevant pages go into context, not the whole wiki.
2. Read those pages (one hop along links, if needed). For "what do we know about…" — **wiki first, then raw sources**, never conversational memory.
3. Synthesize an answer with citations as markdown links: `[Title](wiki/<type>/<slug>.md)` for wiki, `[Description](raw/<concern>/<file>.md)` for raw sources, `[Title](https://...)` for URLs. Confidence is read from structure (the number and independence of sources, related decisions, the presence of counter-arguments), not from numeric scores — and the answer reflects that.
4. If a gap surfaced — propose the next step: a new page, a missing source, a stale claim to recheck.
5. A substantive answer (comparison, retrospective, justification, briefing) — offer a write-back to `wiki/synthesis/`. **Written-back answers accumulate; chat answers evaporate.**

Ordinary queries write no files. Only a write-back (after an explicit "yes") and an explicit ingest change the wiki.
