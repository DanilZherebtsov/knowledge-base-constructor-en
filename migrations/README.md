# Constructor migration feed

**Structural** constructor updates (splitting/merging/renaming mechanics, architecture shifts) cannot be derived by maintenance through a file swap — for those, **ordered step-by-step instructions** live here. Routine updates (`[instruction]`/`[format]`) go as before, with no migration file. Model — ADR-0024 (in the constructor's private meta-repo).

**File format** — `NNNN-<slug>.md`, frontmatter:

| Field | Meaning |
|---|---|
| `id` | Monotonic number (order in the feed). |
| `requires` | Input state: the fingerprint part versions (1.x) or the version (2.x) at which the migration is applicable. |
| `produces` | Output state: the part versions after applying it (1.x) or `version: X.0.0` (from 2.0 — only a change of the first digit). |
| `supersedes` | Migrations this one cancels: a project that needs it does not replay the cancelled ones. |
| `impact: structural` | The `[migration]` label — not a file swap. |
| `applies-to` | The preset(s) it concerns. |

The body — agent-executable steps against the live production repo + a **built-in artifact gate** (the diff must show only the STANDARD layer; code/data/wiki bodies untouched).

**How it is applied (maintenance).** A lagging project replays the shared feed from its own position up to HEAD **strictly in order**: a routine step — by file swap / note migration with consent; a structural one — by executing its instruction. No reordering, no skipping — **except 0006**: a 1.x project with any fingerprint goes straight to [0006](0006-perehod-na-2-0-0.md) (the move to 2.0.0), which cancels 0001–0005. From 2.0 migrations are replayed by step 4 of the "Update check" in `methodology/lint.md` — those whose `produces: version` falls between the project's version and the mirror's. The position is derived from the fingerprint's per-part versions: a project "needs migration k" if its versions satisfy `requires` but not `produces`. Idempotent (an applied migration no longer matches `requires`).

**Discipline (for the constructor developer).** Made a structural change → add a migration file here in the same change (a write-side gate, the twin of the changelog gatekeeper). Without it, downstream runs into a gap.

**English edition note.** Migrations 0001–0002 predate the English edition and exist only in the Russian feed; English-edition projects are assembled at `base@25` or later, so those migrations never match their `requires`. The shared numbering continues from 0003: future structural migrations appear in both language feeds under the same number.
