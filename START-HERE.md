# Start — assembling the project through conversation

The constructor's entry point. When this folder is present but the structure isn't assembled yet (no `wiki/`, no `STATE.md`), and the human simply starts describing what they need — Claude **does not demand an "assemble class X" command**; it runs a short interview with guiding questions and assembles the project itself.

> Naming a class ("assemble saas-product") is the mode for deterministic assembly/testing. The normal entry is conversational, per this file.

## When it fires

First contact in a project where the constructor lies but the structure doesn't — **regardless of what the first message says** (even "hi" or something off-topic). The always-on trigger lives in the constructor's root [CLAUDE.md](CLAUDE.md): it says to greet on the very first message and start this interview, without waiting for the human to bring up the project themselves. No special command needed.

## The interview (guiding questions, one or two at a time, wait for answers)

**Step 1 — what kind of project (determines the STARTING preset).**
Ask "What will you be doing in this project?" and pick a base from the answer:
- building software/a product, code exists or will → **saas-product**;
- research, accumulating knowledge from sources, no code → **research-project**;
- running a business / personal affairs, decisions and counterparties → **business-run-general**;
- fits nothing → "assemble custom" mode (à la carte from [mechanics-catalog.md](mechanics-catalog.md)).

Say the inference out loud and confirm: "Sounds like product development — I'll take **saas** as the base, right?" A preset is a starting point, not a cage.

**Step 2 — additional mechanics (NOT tied to the class).**
Show the relevant optional mechanics from [mechanics-catalog.md](mechanics-catalog.md) in plain words and offer to add them on top of the preset:
- **claim graph** — a graph of claims with evidence and links (usually research; can sit on top of a product too, when the evidence base matters);
- **working with code (`software-engineering`)** — if the project writes, maintains, and deploys its own code (site, landing page, bot, scripts, engine). Provides a code folder in the root, a code-writing cycle (subagent checks → implementation → tests → report), ownership discipline, and, for a web product, an "edits / deploys" role pair. Composes with any preset; with saas it pairs with specs (the spec is the unit of work, this mechanic is the execution). Can be wired in later too;
- etc. per the catalog.

Ask directly: "Will there be your own code — a site/bot/scripts? Anything else?" Add the chosen ones to the preset's mechanics list (on top of the defaults). These operations remain available later — "we'll be writing code" at any moment.

**Don't ask about roles at assembly.** The role machinery exists in every project, always (base machinery, not a class mechanic — ADR-0027); at assembly time roles are usually premature. A role is created with "create role <name>" at any moment after assembly — the human learns this at onboarding (Step 6). Nothing gets deactivated.

**Step 3 — instance data (the regular bootstrap interview).**
"About the project" (what we're building, for whom, constraints), which sources will be coming in, **currency and amount format (ask always — there is NO default,** see the universal question in [base/methodology/bootstrap.md](base/methodology/bootstrap.md)**)**, other domain conventions (units / citation format), if any.

**Step 4 — show the layout in PLAIN language and assemble.**
Before assembling, show the human a short summary to confirm — **in the user's language, no internal jargon** (do NOT say "mechanics / central type / ADR / cascade / layers / version labels / lifecycle"). Say it in human words: what the project is; which capabilities and helpers are on; the key domain facts (currency, roles); and that they just toss materials into `input/` while Claude handles the filing.
> ✗ Jargon: "Base: business-run-general · v8; mechanics decision-lifecycle + roles; central type entities/; STATE 7 sections…"
> ✓ Human: "I'll set the project up for running your business. You'll get: project memory (decisions, counterparties, market facts); helpers — a lawyer, a finance person, a marketer (more can be added); a commitments calendar with a 7-day reminder; amounts in dollars. You toss materials into `input/` — I'll file and absorb them. Assemble?"

Confirmed — assemble per [ASSEMBLY.md](ASSEMBLY.md).

**Step 5 — remove the scaffolding.**
The assembled project `CLAUDE.md` (from `base/`, filled in) goes to the root, **replacing the constructor's root `CLAUDE.md` launcher**. Then delete the constructor tooling (the root launcher is already overwritten; `START-HERE.md`, `ASSEMBLY.md`, `mechanics-catalog.md`, `mechanics/`, `presets/`, `EXPERIMENT.md`, `base/`) — the project keeps only what was assembled (`CLAUDE.md`, `STATE.md`, `methodology/`, working layers, `roles/` — always, the base role machinery). The constructor is scaffolding, not part of the building.

**Tell the human about this step in plain words, no kitchen talk.** "Scaffolding / sandbox / bash / 'I'll load a deletion tool' / no access" is noise for the human — don't talk like that. Deletion needs confirmation — explain the meaning, not the technical cause.
> ✗ "Scaffolding removal via bash is blocked (folder is read-only for the sandbox). I'll load the file-deletion tool."
> ✓ "The project is assembled, the structure is in place. What's left is to remove the temporary files that helped assemble it — deleting them needs your permission."

**Step 6 — a short onboarding.**
Give the human a concise orientation to the assembled project: "How to use it" (the real operations — ingest/query/lint + the class's unit-of-work flow + roles) and "Notes" (stage, lazy layers not yet created, central type, under git or not). Details and exact contents — [ASSEMBLY.md](ASSEMBLY.md), step 9. Only what was actually assembled.

**Mention roles explicitly** (they always exist): "You can spin up a helper for any function at any moment — 'create role <name>' (lawyer, devops, fact-checker — whatever you need). Optionally drop a role description and materials for its work into `input/` beforehand — then it will be created qualified right away, with real context."

## The principle

**A preset is a starting point; the class doesn't dictate.** Any mechanic (claim graph, working with code) can be added on top of any preset — at interview Step 2 or later with a single phrase. The class merely provides a sensible default the human is free to deviate from. Roles are not a mechanic but a base capability: present in every project, always.
