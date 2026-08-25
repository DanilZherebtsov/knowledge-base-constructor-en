# Start — assembling the project through conversation

> **Reading this with your own eyes?** Open a chat in the project folder and type: **`read START-HERE.md and follow it`**. Claude will ask a couple of questions and assemble the project. Any message usually does — this phrase is for when the setup wasn't offered on its own.

The constructor's entry point. When this folder is present but the structure isn't assembled yet (no `wiki/`, no `STATE.md`), and the human simply starts describing what they need — Claude **does not demand an "assemble class X" command**; it runs a short interview with guiding questions and assembles the project itself.

> Naming a class ("assemble saas-product") is the mode for deterministic assembly/testing. The normal entry is conversational, per this file.

## When it fires

First contact in a project where the constructor lies but the structure doesn't — **regardless of what the message says** (even "hi" or something off-topic). The always-on trigger lives in the constructor's root [CLAUDE.md](CLAUDE.md): it says to greet and start this interview, without waiting for the human to bring up the project themselves. No special command needed.

**The moment of first contact doesn't expire.** While the project isn't assembled, offering the setup is the first job in any reply and in any chat, not just the very first one. If the first reply already went out as something else (a greeting the environment generated before `CLAUDE.md` was read, for instance) — offer the setup in the very next one. The "greeted and then waited silently" defect is a silent failure: the human sees an ordinary chat and has no idea a constructor is sitting in the folder. Their fallback is the phrase `read START-HERE.md and follow it`, announced to them in [README.md](README.md) and at the top of this file: it names the file, so it works even where `CLAUDE.md` wasn't picked up at all.

## The language of the conversation — a sweeping rule

**In force from the first message through onboarding, including incidental progress remarks.** Not tied to steps: the ban used to live on steps 4–5 and in onboarding, and it was exactly in the gap between them that "two mutually exclusive central mechanisms" and English-language progress notes leaked out (ADR-0030).

- **The language is the one the human wrote their first message in.** Progress remarks ("structure's assembled, checking it now") are in that language too.
- **Internal words never go out:** mechanic, central type, slot, lifecycle, preset, base, claim graph, wiring, build fingerprint, part versions, file-swap, scaffolding, quality bar, design read (as a term), measurable rule.
- What goes out is only **what the human gets and what they should do**.

> ✗ "decision-lifecycle and the claim graph are mutually exclusive over the central type — each organizes the wiki its own way"
> ✓ nothing: the constructor decides this itself (see [ASSEMBLY.md](ASSEMBLY.md), step 4), the human doesn't need to know
> ✗ "Structure's in place. Now hygiene check for leftover slot markers."
> ✓ "Structure's assembled, checking it now."

## The interview (guiding questions, one or two at a time, wait for answers)

**Step 1 — what kind of project (determines the STARTING preset).**
Ask "What will you be doing in this project?" and offer the options **in this wording and this order** — it describes the person's work, not the wiki's machinery, and the general-purpose option comes first (it is also the answer for "I don't know where this belongs"):

1. **Business / work / affairs (a general-purpose project — fits any kind of work)** → `business-run-general`;
2. **Software / product development** → `saas-product`;
3. **Research work (studies, a thesis, deep research)** → `research-project`;
4. **Something else** → "assemble custom" mode (à la carte from [mechanics-catalog.md](mechanics-catalog.md)).

Do not reword the options and do not reorder them: the person picks by the description of their work, not by a class name. If they answer in free text instead of picking — map that answer onto the same list.

Say the inference out loud and confirm: "Sounds like product development — I'll set the project up for that, right?" The starting base is an entry point, not a cage.

**Step 2 — the code question (the only thing asked about here).**

**Admission rule for the interview (ADR-0030).** A mechanic is put to the human only if it has a **question about an observable fact of their work** — one they can answer on day one, knowing nothing about how the wiki is built. No such question — the mechanic is settled by step 1 against the catalog's criterion, silently. Asking "do you need a claim graph / do you need specs" is **forbidden**: the human will answer "I need everything", and that's noise, not a choice.

Today there is exactly one such question — **working with code (`software-engineering`)**:

> "Will there be your own code — a site, a bot, scripts, dashboards?"

Yes → add the mechanic on top of the preset. It provides a code folder in the root, a code-writing cycle (subagent checks → implementation → tests → report), ownership discipline, and, for a web product, an "edits / deploys" role pair. Composes with any preset; with saas it pairs with specs (the spec is the unit of work, this mechanic is the execution). Can be wired in later too — "we'll be writing code" at any moment.

**The central type is not discussed with the human.** Which unit of knowledge the project needs (claim graph vs the class lifecycle) is inferred from the step 1 answer against the criterion in [mechanics-catalog.md](mechanics-catalog.md) ("Criterion for the central type"). A clash between two contenders is resolved by the assembler itself — [ASSEMBLY.md](ASSEMBLY.md), step 4.

**Don't ask about roles at assembly.** The role machinery exists in every project, always (base machinery, not a class mechanic — ADR-0027); at assembly time roles are usually premature. A role is created with "create role <name>" at any moment after assembly — the human learns this at onboarding (Step 6). Nothing gets deactivated.

**Step 3 — instance data (the regular bootstrap interview).**
"About the project" (what we're building, for whom, constraints), **currency and amount format (ask always — there is NO default,** see the universal question in [base/methodology/bootstrap.md](base/methodology/bootstrap.md)**)**, other domain conventions (units / citation format), if any. Do not ask about future sources: `raw/` starts empty, and subfolders appear at the first "process this".

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
Give the human a concise handoff in plain language, in three strokes: (1) they toss materials into `input/` and say "process this"; (2) each new task is best started in a new chat — knowledge accumulates in the project memory (`wiki/`) anyway, visible from any chat; (3) anytime, `guide` shows the detailed guide to what you can do in this project. **Do NOT list operations line by line in the message** — the detailed list (roles and the class flow included) lives in `HELP.md`, filled at assembly. The exact handoff text — [ASSEMBLY.md](ASSEMBLY.md), step 9.

## The principle

**A preset is a starting point; the class doesn't dictate. But the central type is not the human's choice.** Add-ons (working with code) go on top of any preset — at Step 2 or later with a single phrase; the class merely provides a sensible default the human is free to deviate from. Mechanics that claim the central type (claim graph, spec-/decision-lifecycle), however, do not stack: there is one central type, and it is set by the catalog's criterion against what the human does for a living — not by their answer to a question about a mechanic (ADR-0030). Roles are not a mechanic but a base capability: present in every project, always.
