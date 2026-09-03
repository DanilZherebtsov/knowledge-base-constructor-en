# Maintenance (lint) — wiki health check

Triggered by the phrases: "run maintenance", also "run lint", "check the wiki", "check integrity". The freshness trigger (when to offer a run proactively) lives in CLAUDE.md, §5 of "Discipline".

A periodic check. Two levels with different authority.

## Auto-fix (deterministic — Claude fixes on its own)

- Pages missing from `wiki/index.md` → add with a `(no summary)` stub.
- Index entries pointing at deleted files → mark `[MISSING]` (don't delete).
- Broken internal links with a single same-named file elsewhere → fix the path.
- `sources:` links to moved raw files with a single candidate → fix.
- Missing mutual links between obviously related pages → add.
- A file in `archive/` that is absent from `archive/index.md` → append a line marked `[origin unknown]`: where it came from cannot be recovered, and the stub must show that rather than impersonate a complete journal.
- An `archive/index.md` line with outcome "into archive" pointing at a missing file → mark `[MISSING]` (don't delete). Lines with outcome "deleted" or "kept" are exempt — there is no file for them by construction.

## Report-only (heuristic — requires human judgment)

- Orphan pages with no incoming links.
- Concepts mentioned on several pages but lacking a page of their own.
- Stale statements contradicted by a newer source.
- ADRs with status `active` that contradict a newer ADR.
- Pages over 800 lines.
- Pages without a `sources:` field.
- **A role's slice has fallen behind.** For each role in `roles/` — domain pages that fall into its zone but lack its marking (tag/prefix): collect a proposal list and offer to tag them (the same init pass, [roles.md](roles.md) step 4); never mass-edit, markings go on with confirmation. Catches writes made outside the role chat or before the role existed; role writes are marked at intake (`roles.md`, "Saving"), this check is the backstop for what has accumulated.
- **Depth violation:** any `.md` in `wiki/<type>/<subdir>/...` (pages must sit flat in the type folder). Exceptions — `wiki/index.md` and `wiki/log.md`.
- **The working layers have grown over.** Walk `output/` and `tmp/`: top-level items (file or folder) in `output/` older than **~30 days** → name them as a list with their age and offer **retirement** (the "Retiring into `archive/`" section below). Show the **15–20 oldest plus a tail count** — a list covering the whole folder does not get sorted, it gets scrolled past. **Never delete or move anything yourself.** No candidates — say what was examined ("checked N items, nothing has been left lying around") rather than staying silent: in a mature project an empty `output/` is an anomaly, and silence is indistinguishable from a broken check. A folder's age is that of its **newest** file (while it is still being written to, it is alive). **mtime is a lower bound:** moving the project, a `clone` and git operations reset it, so "everything is fresh" on a non-empty folder is a reason to say the dating looks reset, not to report "clean".
- **Working residue of a long pass has been left lying around.** Subfolders of `tmp/` older than 7 days (progress journals, logs, intermediate chunks) → name them as a list with their age and offer cleanup **or a move into `archive/`**, if what is inside turns out to be not residue but a result worth keeping. **Never delete them yourself, not even here:** normally the pass itself offers cleanup once the result has been accepted (`CLAUDE.md`, "A long pass"), while this check catches runs that never got that far — interrupted, or the human moved on. Since the outcome of the pass is unknown, what's inside may be the only copy of what was done; hence a list and a question, nothing more. An empty `tmp/`, or none at all, is normal — stay silent.
- **Log dates not ascending:** an entry in `wiki/log.md` dated earlier than the previous one → report (the date was likely taken from a file mtime or from when the work was done, not from the environment). Don't sort automatically — whether to fix the date stamp or the entry position is the human's call.

### `STATE.md` hygiene (read silently every session → size = context tax)

`STATE.md` is read at the start of **every** chat, so its growth is a constant tax on context, and status drift distorts any answer to "what's in progress". Everything below is **report-only, never an auto-edit:** STATE holds intentions, and a false positive must not silently erase a plan. The report explains WHY (one phrase about the file's purpose), not just "what". Thresholds are defaults; a large project may deliberately raise them (the numbers live here, in the rule's text).

- **Paragraph-line.** Any `STATE.md` line longer than **~2000 chars** is narrative that belongs in `wiki/` (in STATE — a phrasing + a link, `state-rules.md` §5). Report: the top-N longest lines + their section. Catches the root cause before overall size does.
- **Size.** File > **~30 KB** or a single section > **~10 KB** — report with the number and an estimate of how many tokens that is at each session start.
- **"Done" buffer not collapsed** (`state-rules.md` §6: sits for a week, then → a wiki link / "N tasks, see git log" / removed). Report: entries in "Done" whose date is older than `_Updated:_` by **7–14 days** and still hanging there.
- **Status drift inside STATE.** Items in "In progress" / "Next" carrying a completion marker (`✅`, `completed`, "CLOSED", "ACCEPTED") → should have moved to "Done" or disappeared. Report as a list.
- **Structure.** The class's section set (slot S5) in fixed order; empty ones marked `_empty_`; the `_Updated:_` field present and parseable as a date. _(Don't duplicate the "what to do if the date is stale" logic — the freshness trigger is already always-on in `CLAUDE.md`; here just note the field's presence.)_
- **STATE ↔ wiki/code.** An item in STATE asserting a fact that contradicts a wiki page or the code (`state-rules.md` §7) → flag (don't edit: STATE is not canonical, but it's not the source of truth about facts either).

**Domain checks of the central type.** <<SLOT DOMAIN-LINT: a class with a central type adds its check here. Examples: `architecture/` pages missing the `implementation:` field or with broken paths in it (code drift, saas); a `claim` with status active/validated whose counter-evidence outweighs, and citations without a location (research); overdue commitments from the `STATE.md` "Commitments calendar" (business).>>

## Retiring into `archive/`

`archive/` is for **material we generated that has aged out**: things that settled in `output/` and `tmp/`, lost their relevance, but are not worth deleting outright. It is a different dimension from `raw/`: that one holds the **primary sources** we compile the wiki from, and they are immutable (in a conflict with the wiki, the source wins).

**The human decides, not the check.** Claude collects candidates, shows what it knows about each, and gives a one-line recommendation — but the human picks the outcome. Computing the outcome on its own was tested against live data and does not work: most items simply lack the signal it would need.

**For each candidate show:** what it is (name, type, age), who links to it, and the recommendation. Then one of three outcomes, each **item by item and confirmed**; a blanket "yes, sort it all out" is not accepted — the working layer may have no second copy.

1. **Into `archive/`** — aged out, but throwing it away would hurt: it went outside, it is the only copy, it may have to be produced later. **Function decides, not extension**: a question that went to a person as `.md` is as much a record as a `.docx`.
2. **Delete** — the knowledge is already in the wiki (name the page) or the result is reproducible (name the script or command). Can name neither — this is not outcome 2.
3. **Keep** — the work is alive. Recorded with a date and **not shown again before the next threshold**, otherwise the list never shrinks and people stop reading it.

**What the link search does not see** — say it out loud instead of passing it off as a clean result: pattern-based access (`glob`, `rglob`, reading a whole folder), links without a file name ("the artifact is in `output/`"), mentions inside `.docx`/`.xlsx`/`.pptx`. An unreadable format is **not** "no links". Exclude `wiki/log.md` and `wiki/index.md` from the search area: a journal and a catalogue mention everything that ever went through ingest, and would shield everything.

**Special cases.** A folder older than the threshold — open one level and sort its items, don't move it wholesale. Something that came from outside (a file someone filled in, material that was sent) is not our artifact but a primary source: offer `raw/`, not the archive. A same-stem neighbour (`X` and `X.zip`, `X` and `X_v2`) — call it out as a pair regardless of age and ask which version is the real one.

**Archive rules.**
- Flat, no state subfolders. Don't rename files — renaming breaks the links that still point at them.
- **Secrets and personal data** — check and strip them **before** the move, by the same rule as on the way into `raw/` ([ingest.md](ingest.md)).
- **`archive/index.md` is a retirement journal, not a wiki page** (no frontmatter, outside ingest and outside `page-conventions.md`). One line per outcome, deletions included: `| date | what | from | outcome | closed by (wiki page / command) |`. The line is written **before** the action: otherwise deleting is cheaper than archiving, and cheaper here means more irreversible.
- The archive is read from; nothing is re-sorted or cleaned inside it.
- **Only generated material goes in** — from `output/` and `tmp/`. `raw/` is immutable; `wiki/` uses supersession; in classes with code, `specs/` keep their status in frontmatter. `scripts/` and `data/`, where a class has them, have no cycle of their own — a known remainder, not closed here.

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
3. **Compare per part and show the table** `part | mine | upstream | verdict`. Upstream above mine → that part has an update. Parts the project does NOT carry (mechanics not baked in) — ignore.
  - **Exception — an unwired part the update exists for.** If the entry of an updated part references a mechanic the project does not have, do not drop it silently: name it to the human **as an offer** ("this capability now exists, shall I wire it in?"); they agreed — attach it per the "Attach a capability the project doesn't have" section below. Otherwise they get "some catchers were added" with no explanation of what the catchers are for, and never learn about the update most useful to them. The verdict "no updates" is allowed ONLY if in the shown table every part has `mine == upstream`. Only part versions are compared, never whole files — cosmetics and instance content stay out of the comparison.
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

## Attach a capability the project doesn't have

The project was assembled without some mechanic (`software-engineering`, `design`, …) — it can be attached **at any moment, without reassembly**: mechanics are additive. Triggers: the human says "we'll be writing code" / asks for something visual; the gatekeeper in [ingest.md](ingest.md) caught incoming code or a brand identity and the human confirmed ownership; the catcher in [CLAUDE.md](../CLAUDE.md) resolved the reading in favour of "we own our code"; maintenance named the part as an offer (step 3 above).

**The mechanic's files are not in the project and should not be** — the constructor's scaffolding (`mechanics/`, `presets/`, `versions.json`) is deleted at assembly. The source is the upstream mirror, the same channel as the update check above.

1. **Take a snapshot** — a live read pinned to a SHA, as in step 2 of "Update check". Shell with network: `git clone https://github.com/DanilZherebtsov/knowledge-base-constructor-en.git` into a temp folder **outside the project** (`/tmp/constructor`), so the scaffolding doesn't settle into the repository. Shell without network: `WebFetch` the raw URLs with the SHA in the path, file by file (`mechanics/<name>/…`).
2. **Execute `mechanics/<name>/_about.md`, the "Step-by-step wiring" section** — which files to place, which slots to fill, which lines to add to `CLAUDE.md`. Follow the instruction from the snapshot, not your own idea of what the mechanic is.
3. **Append to the fingerprint** (line 3 of `CLAUDE.md`): add `<name>@<version>`, where the version comes from **that same snapshot's** `versions.json`, not from memory. Without it maintenance doesn't see the part and brings no updates for it.
4. **Note it in `wiki/log.md`**: `## [YYYY-MM-DD] attached: <name>@<version>`.

**Couldn't fetch — say so straight.** Neither the shell nor `WebFetch` produced the files → tell the human in plain words (no "mirror / scaffolding / mechanic"): this capability needs files the project doesn't have, and they can be fetched with one command — then give it (`git clone https://github.com/DanilZherebtsov/knowledge-base-constructor-en.git`), asking them to put the folder next to the project. **A copy of the mirror already sitting next to the project — take it from there straight away**, no network needed: that is the regular path for an air-gapped setup, not a workaround.

**Reconstructing a mechanic from memory is FORBIDDEN.** Writing out "the execution cycle" or "the order of visual work" in your own words without reading the file is a silent substitution: the human sees an attached capability, the fingerprint names a version whose files aren't on disk, maintenance compares **version numbers, not content**, and stays silent. The failure is indistinguishable from success and never surfaces. No files — no attach: a partial attach is worse than none.
