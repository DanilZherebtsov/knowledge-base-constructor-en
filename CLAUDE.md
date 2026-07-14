# CLAUDE.md — template constructor (project not yet assembled)

This project contains the **LLM wiki template constructor**, but the working structure hasn't been assembled yet. On first contact your job is not to answer like a regular chat but to **proactively start the project setup** through a short interview.

## First-contact trigger (ALWAYS-ON)

**On the FIRST message in this project — whatever it says, even "hi" or something unrelated to setup — initiate assembly immediately:**
1. Greet and say in one sentence that this is a constructor and we'll get the project on rails quickly.
2. **Start the interview per [START-HERE.md](START-HERE.md)** — first question: "What will you be doing in this project?".

Don't wait for the human to bring up the task themselves. If they described the project right away — all the better, get to it; if they wrote something unrelated — gently offer to start the setup and ask the first question.

**The "not yet assembled" tell:** the root contains `base/`, `mechanics/`, `presets/`, `START-HERE.md` (the constructor scaffolding) but no assembled project (`wiki/`, `STATE.md`, a class `methodology/`). While that's the case — open every new chat by offering the setup (if the human is already mid-interview — continue it, don't restart).

## Constructor map

- **[START-HERE.md](START-HERE.md)** — the interview and mechanic selection (the front door).
- **[ASSEMBLY.md](ASSEMBLY.md)** — the mechanics of assembling a project from `base/` + a manifest + mechanics.
- **[mechanics-catalog.md](mechanics-catalog.md)** — the registry of detachable mechanics (claim graph, lifecycles, code).
- `base/` — the skeleton; `presets/` — class manifests; `mechanics/` — drop-in mechanics.

## After assembly

The assembled project gets **its own** root `CLAUDE.md` (from `base/`, filled in) — it replaces this launcher file. The constructor scaffolding (`base/`, `mechanics/`, `presets/`, `START-HERE.md`, `ASSEMBLY.md`, `mechanics-catalog.md`, `EXPERIMENT.md`, this file) is deleted — see [START-HERE.md](START-HERE.md), step 5.
