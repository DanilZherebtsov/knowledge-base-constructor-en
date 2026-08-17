# CLAUDE.md — template constructor (project not yet assembled)

This project contains the **LLM wiki template constructor**, but the working structure hasn't been assembled yet. On first contact your job is not to answer like a regular chat but to **proactively start the project setup** through a short interview.

## Setup trigger (ALWAYS-ON, while the project isn't assembled)

**The "not yet assembled" tell:** the root contains `base/`, `mechanics/`, `presets/`, `START-HERE.md` (the constructor scaffolding) but no assembled project (`wiki/`, `STATE.md`, a class `methodology/`).

While that tell holds, offering the setup is your first job **in any reply and in any chat**, not just the first one:

1. Greet and say in one sentence that this is a constructor and we'll get the project on rails quickly.
2. **Start the interview per [START-HERE.md](START-HERE.md)** — first question: "What will you be doing in this project?".

Don't wait for the human to bring up the task themselves: whatever their message says — "hi", something off-topic, a ready description of the project — steer to the setup (described the project right away — all the better, get to it).

**If the first reply already went out as something else** — a greeting, an answer to an off-topic message, an offer to "take a look at what's in the folder" (including one the environment generated before you read this file) — offer the setup **in your very next reply**, without waiting to be asked. First contact doesn't expire: it lasts until the project is assembled. If the human is already mid-interview — continue it, don't restart.

## Constructor map

- **[START-HERE.md](START-HERE.md)** — the interview and mechanic selection (the front door).
- **[ASSEMBLY.md](ASSEMBLY.md)** — the mechanics of assembling a project from `base/` + a manifest + mechanics.
- **[mechanics-catalog.md](mechanics-catalog.md)** — the registry of detachable mechanics (claim graph, lifecycles, code).
- `base/` — the skeleton; `presets/` — class manifests; `mechanics/` — drop-in mechanics.

## After assembly

The assembled project gets **its own** root `CLAUDE.md` (from `base/`, filled in) — it replaces this launcher file. The constructor scaffolding (`base/`, `mechanics/`, `presets/`, `START-HERE.md`, `ASSEMBLY.md`, `mechanics-catalog.md`, `EXPERIMENT.md`, this file) is deleted — see [START-HERE.md](START-HERE.md), step 5.
