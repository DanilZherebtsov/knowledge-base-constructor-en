# Mechanic: design

**What it does (1 line):** gives the project **the visual competence** — how the things people look at get made and accepted (landing pages, sets of graphics, presentations): a one-line design read of the task, directions derived from the audience's world instead of the category default, a quality bar with rules you can measure, bounded acceptance with a fresh lens and honest degradation, and a brand identity accumulated in `BRAND.md` from what was actually built. It does **not** take the wiki's central type; it composes with any lifecycle and with `software-engineering`.

**Wired into ALL presets by default** (ADR-0038). The interview does not ask about it: "will you be making things people look at" gets a yes almost every time and therefore discriminates nothing. It is a separate part in `versions.json` rather than a chunk of base because its list of category defaults goes stale faster than anything else and must be able to bump without dragging the base version along.

## Target-project slots it touches

- **DESIGN-PTR** (in `CLAUDE.md`, "Wiki: page types and operations", the paragraph "Make a presentation / landing page / cover / diagram / layout") — *fills.* The paragraph already exists in base, written for the "mechanic not wired in" case; the mechanic **replaces** the slot content with a pointer to `design.md`. Append nothing alongside it — a second visual paragraph leaves an always-on statement in the root file saying the competence is absent.
- **the always-on order line** (in `CLAUDE.md`, "How Claude works on tasks → the act phase") — *inserts* a pointer to the order of work on visual things, which would otherwise only be read on trigger.
- **OWNED-DESIGN** (in `methodology/ingest.md`, the gatekeeper's "incoming brand identity" branch) — *fills.* Replaces the placeholder with a pointer: work with someone else's brand identity is run by `design.md`.
- **HELP-OPS** (in `HELP.md`) — *extends* with the action phrase "make a presentation / landing page / cover".
- **S2** — *relies on* `decisions/` and `principles/` existing (brand-identity decisions → ADR, acceptance lessons → principles).

## Target-project files it touches

- `methodology/design.md` — this drop-in is placed there (the mechanic's only file).
- `CLAUDE.md` — three points: the visual branch in the dispatcher; the `BRAND.md` line in the "Architecture" tree; the always-on order line in "the act phase". Plus a line in "Documents and naming → Where things go".
- `methodology/ingest.md` — fill the OWNED-DESIGN slot.
- `HELP.md` — the HELP-OPS slot.
- `methodology/bootstrap.md` — `BRAND.md` is not created empty.
- May create during work (not at install time) — `BRAND.md` at the root **only by the human's consent**, offered once after the first visual piece of work is accepted; plus an ADR about the chosen direction in `decisions/`, principles distilled from acceptance passes.

## Which mechanics it depends on

- **the base skeleton** — the dispatcher paragraph in `CLAUDE.md`, the gatekeeper in `ingest.md` (the OWNED-DESIGN slot), `wiki/{decisions,principles}/`, the injection point in "the act phase", the long-pass rule (`base@30`) for sets and multi-slide decks. Without them the mechanic has nothing to attach to.
- **`software-engineering`** — an **optional companion mechanic.** Present — there is one cycle, its own (see "Composition"). Absent — the mechanic works standalone, without tests and deployment. This matters: presentations and graphics are made first and foremost by classes without code.
- **roles** — base machinery, always present. The mechanic ships **no** role samples of its own: the fresh acceptance lens is conveniently framed as a role, and `create role design reviewer` works in any project without shipping a sample.

## Composition

- **With `software-engineering` — composes, and there is ONE cycle.** Two axes: code owns files and execution, the visual owns the decision and acceptance by eye. The visual mechanic does not open a second statement gate and adds no subagent rounds — it inserts the design read into the statement and a visual pass into the acceptance criteria. Without this rule a real landing-page task doubles the subagent count (the lesson of ADR-0034).
- **Does not take the wiki's central type** — a superstructure, not a foundation; compatible with any lifecycle.
- **Does not take `DOMAIN-LINT`** — it brings no lint checks of its own.

## Step-by-step wiring

1. **Copy the drop-in:** `design.md` → `<target>/methodology/design.md`.
2. **Fill the OWNED-DESIGN slot** in `methodology/ingest.md`. Replace `<<SLOT OWNED-DESIGN: …>>` with: "handling incoming brand identity, carrying it into `BRAND.md`, and all further work on visual things is run by [design.md](design.md)".
3. **Fill the DESIGN-PTR slot** in the dispatcher paragraph (`CLAUDE.md`, "Wiki: page types and operations", the paragraph "Make a presentation / landing page / cover / diagram / layout"). Replace `<<SLOT DESIGN-PTR: …>>` with: "Follow [methodology/design.md](methodology/design.md)". **Do not append anything alongside it:** the paragraph already exists in base, written for the "mechanic not wired in" case, and a duplicate leaves an always-on statement in the root file saying the visual competence is absent.
4. **Add `methodology/design.md` to the "Architecture" tree** — into the `methodology/` block alongside the other files; otherwise the file is invisible in the project map. In the same place, **declare `BRAND.md`**: "`BRAND.md` ← What the things we make look like: palette, faces, rhythm, named rules, the project's ban list. Edited by the human. Created only by the human's consent — offered after the first visual piece of work is accepted; from what was built, never in advance. Details — [methodology/design.md](methodology/design.md)." In parallel, add to "Documents and naming → Where things go": "decisions about the look → `BRAND.md` (not `wiki/`)".
5. **The always-on order line.** In "How Claude works on tasks → In progress — the act phase", after the long-pass rule, add: "**Visual things go only through the order of work on the visual**: a one-line design read → what is already true → directions to choose from (build not your own first) → build with the quality bar → acceptance in two batches with a fresh lens → record in `BRAND.md`. In full — [methodology/design.md](methodology/design.md)." Without it the order is read only on trigger, and Claude starts drawing out of habit first.
6. **HELP-OPS.** The visual phrase is already listed in the slot's own class-specific block — do not word it separately; just make sure it is written into the assembled `HELP.md` when `HELP-OPS` is filled.
7. **Bootstrap — verify, do not add.** The `BRAND.md` item in "What is NOT created at bootstrap" arrives from base (`base@33`) and is already worded in full. Confirm it is there; adding your own means a duplicate.
8. **What NOT to add.** Do not create `BRAND.md` at bootstrap. Do not turn it into a wiki type and do not create a `design/` folder — that is a structural change, and a root-level file is not one. Do not ship role samples. Do not duplicate the quality bar into the class's lifecycle file.

## Deactivation

The mechanic is on in every preset; there is no routine deactivation. If a project deliberately declines the visual competence — do not copy `methodology/design.md`, leave the OWNED-DESIGN slot in its base form, and do not insert the dispatcher branch, the `BRAND.md` line, the always-on order line, or the HELP-OPS phrase. The build provenance line then does not list the mechanic.

## Attaching later (a project assembled before this version)

1. **Trigger:** the human asks for something visual; OR the gatekeeper in `ingest.md` caught an incoming brand identity; OR maintenance saw an upstream part the project does not have.
2. **Action:** run wiring 1–7 against the live project. The mechanic is additive and needs no migration: it creates no new folders, and `BRAND.md` is a root-level file created by the human's consent during work.
3. **Source of the files**, if the constructor scaffolding has been removed, is the upstream mirror (the same channel maintenance uses to compare versions).

## The debt this mechanic carries

**The "Examples as of" section in `design.md` is dated and must be revisited at every bump of `design`.** It consists of specific faces, palettes and devices — those are the tells of their year, not eternal rules; the normative part lives in the guessability test. The workshop takes this debt on knowingly: concrete tells work noticeably better than abstractions, but they age faster than anything else in the constructor.
