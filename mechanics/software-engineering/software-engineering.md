# Software engineering — the project writes, maintains, and deploys its own code

Attached to classes that **work with their own code** — writing it from scratch, maintaining existing code, deploying (a landing page, a site, scripts, a bot, an app, a backend engine). Carries **all code-work competence** in one place: the code-writing cycle, a home for the code and ownership of it, secrets discipline, reproducible deployment, a pair of product roles.

Composes with **any** class lifecycle. For saas it works paired with `spec-lifecycle`: the spec gives the **unit of work** (what we do and how we track it), this mechanic gives the **execution** (how code gets written and verified). A class without a spec cycle (a landing page, business/research tooling) gets the same cycle and ownership without a mandatory spec. The mechanic does **not** claim the central wiki type — it is an add-on, not a foundation.

## The code folder — root-level, named for what it is

- Working code lives in **a root folder named for what it is** (`landing/`, `src/`, `scripts/`, `bot/`) — **not** in `raw/` (a read-only archive there) and **not** in `output/` (disposable working files there).
- The folder is created **by human decision** at the moment the project **first gets code** — via two paths: *working code arrives* (the gatekeeper in [ingest.md](ingest.md)) **or** *it is decided to build code from scratch* (a request like "make a site / bot / app" while there is no code folder yet). Both paths lead to the same branch: create the folder + (per the gate below) offer the roles. The folder is not created empty at initialization (bootstrap).
- Several heterogeneous assets — several root folders (`landing/`, `bot/`), each named for what it is.

## Source of truth — the files on disk

- **Code beats the wiki.** For the code itself the truth is the files in the code folder, not their retelling in `wiki/`. On divergence — trust the disk.
- Only **knowledge about the code** goes into `wiki/`: what it is, its purpose/offer, key facts, accepted decisions. The code itself, configs, and runtime files are not copied into the wiki.
- **How-to lives in the code folder, not the wiki:** how to build, how to deploy, what to check before launch → `README.md` next to the code. The wiki is a digest of knowledge, not a tech manual.
- **Runtime data lives in `data/`, not `wiki/`.** Prompts, KB dictionaries, config files, templates live in the code layer (`data/`); the runtime consumes them, Claude does not read them as knowledge. Only the **contract** (schema + how it is updated) goes into `wiki/`. Putting a runtime file into `wiki/` is a classic mistake.
- An architectural/infrastructure **decision** (stack, hosting, domain, deploy method) → an ADR in `decisions/` through the class lifecycle. A **lesson** ("we don't do that anymore") → `principles/`.

## The execution cycle (for changes to logic or behavior, not minor edits)

A change that touches the product's logic or behavior is written not "straight by hand" but through a **fixed cycle with independent review gates**. Steps are not skipped. "Independent review" = separate subagents (Agent/Task), each with its own context, not seeing each other's verdicts; each one's instruction — to look for *what's wrong*, not to confirm.

**How to invoke it (for the human).** No need to enumerate the steps — "check with agents, re-check, report": the cycle is mandatory by default. Naming the task is enough ("take task X" / "do X" / "let's implement X, it's a sprint"), Claude unrolls the cycle on its own. If the class runs `spec-lifecycle`, the cycle's input is the **spec**; if not — a short task statement (problem, what changes, acceptance criteria).

1. **Task statement before code.** No code is written until there is an explicit task statement: the problem, what changes, how we implement it, **acceptance criteria**. For a **request that is exhaustive by nature** ("the entire regulatory base", "all integrations", "every type of X") — the statement enumerates the **full list of elements** explicitly, not with one summarizing phrase: otherwise excluding an element becomes invisible both to the gate and to the human. In a class with `spec-lifecycle` the statement is a **task spec** (`specs/...`); without it — a formulated task in `STATE.md`/chat. (A trivial edit — a typo, a rename, a comment fix — needs no statement and no cycle; the cycle kicks in as soon as the task grows into an explicit statement.) **First play the statement back to the human** in plain words (how you understood the task, what changes and where, what result they will see, where you filled gaps with assumptions) and get an explicit "yes" — and only then the subagent gate (step 2). Subagents check the statement for correctness, but they inherit your reading of the task and do not catch divergence from what the human meant — only the human catches that (see "How Claude works on tasks → Gate before implementation").
2. **Task statement gate — ≥4 independent reviews (before implementation), in parallel:**
   - **3 subagents — correctness and completeness:** does the statement solve the stated problem, are any requirements missing, are the acceptance criteria realistic;
   - **1 subagent — impact on other modules:** what else in the system is affected, which contracts/invariants/call sites break, where the regressions are (input — the system knowledge in `wiki/`, for saas — `architecture/`); **flags irreversibility and blast radius** (pinned forever, silent data corruption, breaks consumers' contracts).

   Findings are consolidated → the statement is revised (the spec is mutable while `active`) → the gate **repeats** until the review is clean. **Gate depth follows risk, not one size by default:**
   - **Cosmetic / isolated additive** (does not touch logic, contract, or data; a self-contained piece with no connections) — **2 lenses** (one correctness, one impact), a single pass. A full cycle over text and styling is the same overspend as a gate skipped on a risky change.
   - **Routine local edit** — the base **3+1**.
   - **Irreversible / wide-impact** — a strengthened gate: **+one adversarial round** aimed at disproving safety. The strengthening is triggered by a **concrete flag** from the impact reviewer (pinned forever, silent data corruption, breaks consumers' contracts), **not** by a task being "about deployment" — otherwise the strengthened path becomes the default for the whole sprint.

   The number of reviews is tied to the **surface** (how many distinct modules/lenses actually need checking), **not to repetition**: a genuinely wide task legitimately gets more lenses, but multiplying identical verifiers over the same piece is not quality — it is noise and a queue (you hit the concurrency limit and session limits). If the gate does not converge in ~two rounds, that is a signal that the **statement or the recon under it** is defective: fix that, don't add rounds.
3. **Implementation** — strictly by the accepted statement.
4. **Implementation gate — a repeat check (after the code):** same structure — correctness + module impact. Correctness is verified **by execution**: run the tests and acceptance criteria (no tests — write and run them), reproduce the behavior, not just re-read the code. Agent review sits on top of the run, not instead of it; "no regressions" is confirmed by the same run over the affected modules.
5. **Report.** Claude reports the result **in the user's language** (what was done, what was verified, where we deviated from the statement) — see "How Claude works on tasks → In the human's language". If the statement enumerated the elements of an exhaustive list (step 1), the report shows the outcome for each one (done / deferred with reason) rather than collapsing them into one generic backlog line.

**Loop on errors.** An error at any step → the task returns to the start of the cycle and goes through it again until the implementation is error-free. If it does not converge in a reasonable number of passes — that is a **blocker** (below), not an infinite loop.

**Gap inside scope.** It turns out the statement missed something that **falls within the task's scope** → write it into the statement, implement in the same cycle (that is exactly why the active spec is mutable).

**Blocker inside scope.** An element is listed in the statement's enumeration (see step 1), but it cannot be closed right now for a concrete reason — typically "nothing to verify against" (no test data for that category) → **not a license to quietly move it to the backlog**. First — try to remove the cause yourself (for missing data — construct a representative test sample). Failed → surface it in the report as an explicit item: what is not covered, why, options (send a sample / accept it unverified / consciously exclude it) — and wait for the human's decision on each such item. **The spec/task does not go to `completed` until every element of the exhaustive list has an outcome** — "done" or "the human explicitly excluded it, with the reason recorded in the spec". There is no silent "the rest sometime later"; parceling such items out as one-liners into STATE "Next" is exactly the silent disappearance this rule protects against.

**Blocker outside scope.** The task runs into work **beyond its own scope** (e.g., another module needs reworking) → **don't drag it in silently**: surface it to the human, propose options (a separate stand-alone task / a separate sprint), record it in `STATE.md` "Blockers and risks" (and in the spec's/sprint's `blocked_by:`, if `spec-lifecycle` is in use). The human's decision spawns a new unit of work.

> **A sprint plan gets the same gate.** If the class runs sprints (`spec-lifecycle`), the sprint plan passes the **task statement gate before tasks start** (the same mechanism, step 2): is the decomposition right, are the design conditions and assumptions too broad, what the task bundle affects as a whole. Plan-level defects are cheapest to catch here — before the first line of code.

## Secrets discipline

- Secrets (`.env`, keys, tokens, configs with credentials) — only in a file under `.gitignore`. **Never** in `wiki/`, in git, or in chat.
- Any config with credentials goes into `.gitignore` **before** a secret is written into it.

## Code changes

- Targeted edits — the "Targeted changes" rule from `CLAUDE.md` applies to code too.
- **Re-read a shared file from disk before editing it:** it may have been changed in another chat or role. Edit on top of the current version, not on top of a copy from memory.
- Deployment is reproducible: one command, documented in the code folder's `README`.

## Product roles — an offer on demand (gate: deployable web product)

The two roles form a "one edits — the other publishes" pair. This axis exists **only for a deployable web product** — a site, a web service, a bot. So the offer is gated by code type:

- **Deployable web product** (site / web service / bot) — when the code folder is created (code arrives, or it is decided to build from scratch), **offer** the human two roles (by consent, like any role — [roles.md](roles.md)):
  - **who edits the product code** → sample [roles/_developer.md](roles/_developer.md);
  - **who deploys and operates it** → sample [roles/_release-manager.md](roles/_release-manager.md).
- **Any other code** — a mobile/desktop app, a library, a CLI, an ML model, a data pipeline, a one-off script, etc. — do **not** offer the roles. The gate's default is **silence**; the offer fires only for the explicitly deployable web product above. The "edits/deploys" pair and its samples are tuned for web publishing (hosting, domain, SSL, forms) and will not fit other code. The code folder and the conventions above still apply; a role — only on demand via the usual "create role" → `_template.md`, if one is really needed.

Agreed — the usual "create role" flow starts ([roles.md](roles.md)), and **the named sample serves as the starting draft description** (step 1 of the flow). The role→sample binding is hard — no guessing. Other roles (beyond these two) follow the `roles.md` default → `_template.md`. The two roles work as a pair; the handoff protocol is in the samples themselves.

> Roles exist in every project (base machinery, ADR-0027), so the `_developer`/`_release-manager` samples are always available. They are installed not because roles exist (they always do) but via the **deployable-web-product gate** above: for other code the "edits/deploys" pair is not offered.
