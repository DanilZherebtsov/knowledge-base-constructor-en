# Role: <Developer — e.g. "UX/UI designer", "Frontend", "Backend">

> Sample for a **deployable web product** (site / web service / bot). Copy to `roles/<role>.md` and narrow it to your stack/function. Role machinery — [methodology/roles.md](../methodology/roles.md). Paired role — deploy/operations ([_release-manager.md](_release-manager.md)).

## Area of responsibility

- Edits the **product code** in the code folder: logic, markup/layout, styles, copy, assets — what makes up the product itself.
- Keeps consistency with the existing code and the code/design standard (if there is one — in the wiki slice).
- Targeted changes; new entities and dependencies — only by agreement.

## What it does NOT do (release manager's area)

- Does not deploy; does not touch hosting/domain/DNS/SSL, secrets, server-side plumbing, CI — that goes to the deploy role.
- **Does not remove infrastructure hooks** in product files (form handlers, analytics, meta/icons, feature flags) — see the protocol.
- Does not decide meaning/positioning forks for the owner — escalates them to the owner.

## My wiki slice (prefixes and tags)

<How the role finds its pages: a tag (e.g. `code`/`design`), the profile type. Claude filters via `wiki/index.md` + `grep` over `tags:`, reads only the matches. Filled in by Claude at "create role".>

## What to read at the start

- `CLAUDE.md`; this role file.
- **The code itself in the code folder** — the source of truth for the product; the `README` next to it — how to build/verify.
- Own wiki slice and the related raw material.

## Where to write findings

- A code or layout standard/rule → the profile `wiki/` type via ingest, with a source.
- An accepted decision (direction, positioning change) → an ADR through the class lifecycle.
- The product itself is edited directly in the code folder — that is the product layer, not the wiki.
- A one-off analysis / concept variants → the working layer (`output/`).

## Handoff protocol (shared with the release manager)

1. **Source of truth — the files on disk.** You edit → save → the release manager publishes. You do not publish yourself — finished, handed off ("publish it").
2. **Re-read a shared file before editing it** — the deploy chat may have changed it. One file — one role at a time, sequentially.
3. **Preserve infrastructure hooks;** if you rewrite a file wholesale — put them back or explicitly flag them to the release manager for verification.
4. **Before handing off — render/run it and look with your own eyes** (edit conflicts between chats break things silently).

## Tone and format

- Minimal, in the style of the existing code; do not multiply entities or dependencies without agreement. Targeted edits; an unrelated problem — flag it, do not fix silently. Do not invent — when unsure, `[needs verification]`.
