# Maintenance (lint) — wiki health check

Triggered by the phrases: "run maintenance", also "run lint", "check the wiki", "check integrity". The freshness trigger (when to offer a run proactively) lives in CLAUDE.md, §5 of "Discipline".

A periodic check. Two levels with different authority.

## Auto-fix (deterministic — Claude fixes on its own)

- Pages missing from `wiki/index.md` → add with a `(no summary)` stub.
- Index entries pointing at deleted files → mark `[MISSING]` (don't delete).
- Broken internal links with a single same-named file elsewhere → fix the path.
- `sources:` links to moved raw files with a single candidate → fix.
- Missing mutual links between obviously related pages → add.

## Report-only (heuristic — requires human judgment)

- Orphan pages with no incoming links.
- Concepts mentioned on several pages but lacking a page of their own.
- Stale statements contradicted by a newer source.
- ADRs with status `active` that contradict a newer ADR.
- Pages over 800 lines.
- Pages without a `sources:` field.
- **Depth violation:** any `.md` in `wiki/<type>/<subdir>/...` (pages must sit flat in the type folder). Exceptions — `wiki/index.md` and `wiki/log.md`.
- **Log dates not ascending:** an entry in `wiki/log.md` dated earlier than the previous one → report (the date was likely taken from a file mtime or from when the work was done, not from the environment). Don't sort automatically — whether to fix the date stamp or the entry position is the human's call.

**Domain checks of the central type.** <<SLOT DOMAIN-LINT: a class with a central type adds its check here. Examples: `architecture/` pages missing the `implementation:` field or with broken paths in it (code drift, saas); a `claim` with status active/validated whose counter-evidence outweighs, and citations without a location (research); overdue commitments from the `STATE.md` "Commitments calendar" (business).>>

## Update check (by build fingerprint)

The project was assembled from the constructor and carries a **fingerprint** on line 3 of `CLAUDE.md`: which part versions it was assembled from (`base@N · <mechanics>@N · <manifest>@N`).

**This block is UNCONDITIONAL: it runs in full on EVERY lint run, regardless of the wiki's state.** Whether the wiki is empty, freshly bootstrapped, unchanged since last time, the N-th lint in a row, health is clean — none of that enters the condition AT ALL: upstream (the constructor) evolves independently of your wiki. Excuses like "nothing changed", "can skip it", "already checked", "same wiki" are forbidden; that is precisely the failure this block was made mandatory against.

**Without two artifacts the check is NOT done** — don't state an update verdict and don't append to `wiki/log.md` until your reply contains: (a) the pasted body of the `versions.json` response; (b) a per-part comparison table with the numbers. "Re-checked, no updates" without them is an empty claim, not a result.

1. **Read your own fingerprint** (line 3 of `CLAUDE.md`) — the `part@version` list.
2. **Fetch fresh versions with a live read (not from memory / a previous output) and PASTE its output into the reply. Only pinned to a commit (SHA), never the `main` branch.** If the shell has network — two steps:
   ```bash
   SHA=$(git ls-remote https://github.com/DanilZherebtsov/knowledge-base-constructor-en.git HEAD | cut -f1)
   curl -s "https://raw.githubusercontent.com/DanilZherebtsov/knowledge-base-constructor-en/$SHA/versions.json"
   ```
   **Shell without network (sandbox) — the same two steps via `WebFetch`** (a live GET that bypasses the local sandbox, so it works where `curl` is blocked): first `https://api.github.com/repos/DanilZherebtsov/knowledge-base-constructor-en/commits/main` → take the `sha` field; then `WebFetch` the raw URL with that SHA in the path.
   **Why by SHA and not by `main`:** `raw/main`, the contents API, and the `WebFetch` cache are all edge caches; reads of a mutable branch diverge, and there is no way to pick the right one (real cases: the contents API returned 12, raw returned 8, the truth was 14). A URL with the SHA in the path is **content-addressed** — no cache can return foreign bytes under that SHA; the snapshot is coherent and current, and the "which read to trust" divergence disappears by construction. A slightly stale SHA is fine — it yields a coherent snapshot, not a mix of numbers from different caches.
   **`WebSearch` is FORBIDDEN for this check.** The search index serves a days-old snapshot — it is what produced a phantom `base:12` against a fresh upstream. Only a live GET: `curl` or `WebFetch`.
   **The "upstream" number counts ONLY if it arrived via a SHA-verified live read.** Neither the shell nor `WebFetch` worked — the check is NOT done: say exactly that ("couldn't reliably check upstream"); do NOT put a number from cache/search/memory into the table, and do not output a "matches" verdict.
3. **Compare per part and show the table** `part | mine | upstream | verdict`. Upstream above mine → that part has an update. Parts the project does NOT carry (mechanics not baked in) — ignore. The verdict "no updates" is allowed ONLY if in the shown table every part has `mine == upstream`. Only part versions are compared, never whole files — cosmetics and instance content stay out of the comparison.
4. **For each updated part — figure it out yourself** (an internal assessment, not output for the human): read its entry in the upstream `CHANGELOG.md` — **a single file in the mirror root**, heading `## <part>@<version>` (e.g. `## base@19`, `## spec-lifecycle@2`). Read it **at the same SHA pin as step 2** (the same commit) — the version and its description come from one snapshot, ruling out the "number newer than description" cache skew. For a class manifest — the content of `presets/<class>.md`. Classify the blast radius: *`[instruction]`* (prose/rule) — fixed by file edits, knowledge (`wiki/`) untouched; *`[format]`* (a new field/section on pages) — requires a `wiki/` migration.
5. **Report to the human — in plain language, no IT jargon.** The changelog is written for a developer — don't retell it verbatim; translate it into substance. For each updated part the human must understand three things:
   - **What changed** — 1–2 sentences in terms of benefit, not mechanics.
   - **Does it affect them** — either "nothing to touch, this is about how I work" (`[instruction]`), or "your notes will gain <what exactly> — can be filled in gradually" (`[format]`).
   - **Take it or not** — a one-sentence recommendation.

   In the reply to the human do NOT use: "fingerprint / version / base@N / mechanic / manifest / migration / lazy-eager-skip / `[format]`-`[instruction]` / blast radius / API".
   > ✗ Jargon: "base@12: `[instruction]` Flow B now goes through the contents API, raw cache eliminated, no migration needed."
   > ✓ Human: "I picked up an improvement to how I check for updates: I sometimes compared against a slightly stale list and could miss a fresh update — now I always look at the most current one. Doesn't touch your notes, nothing to do. I'd take it — it's simply more reliable."

   Then **offer to apply**: `[instruction]` — surgically, with confirmation; `[format]` — name the scope in human terms ("a field will be added to roughly N notes") and the choice: **(a)** fill in as we go, whenever we open a note — the default; **(b)** sweep through all of them now; **(c)** postpone. Never mass-rewrite notes without consent. Applied — **bump that part's version in the fingerprint** (line 3).

After lint — append to `wiki/log.md`:
```
## [YYYY-MM-DD] lint | found N issues, fixed M
```
