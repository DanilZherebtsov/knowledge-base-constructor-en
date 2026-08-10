# The independent review gate — before doing the work

One mechanism for every class: **before a nontrivial task is executed, its task statement passes several independent reviews**. "Independent" = a separate subagent with its own context; reviewers **do not see each other's verdicts**; each one's instruction is to look for *what is wrong*, not to confirm what has already been decided.

Read on trigger from `CLAUDE.md` ("How Claude works on tasks"). With the code mechanic attached, that mechanic builds its execution cycle and the gate's second half on top of this one (a re-review after the code is written, correctness through an actual run).

## What the gate catches, and what it does not

**Catches:** a missed requirement, an unrealistic acceptance criterion, a neighbouring area disturbed, an unnoticed irreversibility.

**Does not catch divergence from the human's intent.** Reviewers inherit your reading of the task and will be wrong together with you. That is why "confirm understanding" (`CLAUDE.md`) comes **before** the gate, not instead of it: first the human confirms the task was understood correctly, and only then is the statement itself reviewed.

**How well reviewers are grounded depends on the class.** Where there is code, a claim can be checked by running it. Where there is no code (a decision, a contract, a research conclusion), the reviewer leans only on the statement and on `wiki/`, and there is nothing to check the reviewer against. So there the gate's output is **material for the human**, not a verdict of "clean": a widening of the view and support for the decision, not verification. Report it in exactly those words; do not pass it off as verification.

## If you can settle it by doing it, settle it by doing it

Lenses do not replace a run. Where there is a way to **check** (run a test, recompute, compare against the source, ask the counterparty), that is primary — cheaper and more reliable than any discussion. The gate is for what cannot be settled that way, or cannot be settled yet.

Without this rule the gate turns into a debate about something that resolves in ten seconds.

## When it engages

**Engages** when the task is nontrivial (it needs a choice between approaches, or a plan of several steps) **and** its result is worth something: it changes the product's behaviour, spends money, goes outside, or lands in `wiki/` as canonical.

**Does not engage** on the trivial (a typo, a rename, restating a page) or on the cheap-and-reversible. In doubt — treat the task as nontrivial, but take the lower depth.

## Depth by risk, not one size for everything

| Risk | Lenses | Which ones |
|---|---|---|
| **Cosmetic / isolated additive** — does not touch logic, money, obligations, or data; a self-contained piece with no connections | **2**, a single pass | one on correctness, one on impact |
| **Routine local work** | **3 + 1** | three on correctness and completeness, one on impact on neighbouring areas |
| **Irreversible or wide in impact** | **3 + 1 + an adversarial round** | plus an adversarial pass: prove the decision does harm |

A full cycle over text and styling is the same overspend as a gate skipped on a risky change.

**What counts as irreversible.** It cannot be undone, or undoing it is expensive: a signature and a financial commitment; anything sent outside (an email, a publication, data handed to a third party); deletion or overwriting with no copy; anything pinned for a long time (a tariff, a domain, a data schema, a public contract); silent data corruption.

The strengthening is triggered by a **concrete flag** from the impact reviewer — a named item from the list above, not a general sense that "the task is important". Otherwise the strengthened path becomes the default for everything, and the human starts routing around the gate entirely.

## What the reviewers are given

- **Correctness and completeness** — does the statement solve the task as posed; is a requirement missing; are the acceptance criteria realistic.
- **Impact on neighbouring areas** — what else in the system or in the business is affected; which obligations, agreements, contracts, invariants break; where the regressions are. Input — the knowledge in `wiki/`. Flags irreversibility and blast radius (see above).
- **Disproof** (strengthened contour only) — the instruction is not "assess" but **disprove**: produce a scenario in which this decision does harm. Found nothing — say so; silence is a result too.

**A request that is exhaustive by its nature** ("all the contracts", "every type of X", "the whole body of regulations") — the statement enumerates the items as an **explicit list**, not as one covering phrase. Otherwise a dropped item is invisible both to the reviewers and to the human.

## Reconciling the findings

**Every finding gets an explicit outcome** — accepted (what exactly changes) or rejected (why). Collapsing three objections into one line, restating an inconvenient one more softly, or quietly dropping it — forbidden. This is a floor rule: it always applies and costs nothing.

Findings reconciled → the statement is revised → the gate **repeats**, until it comes back clean.

## When to stop

**The number of lenses is tied to the surface, not to anxiety.** However many distinct areas genuinely need looking at — that many lenses. A genuinely wide task legitimately gets more; multiplying identical reviewers over the same piece is not quality, it is noise and a queue.

**Not converging in ~two rounds means the statement itself, or the recon under it, is defective.** Fix that, don't add rounds.

**There are no numeric agreement scores or stopping thresholds.** "Agreement 0.8" looks like a measurement and is an invention — "Discipline", §3. What stops the gate is a structural signal (clean / statement defective), not a number.

## Reviewers write nothing

A subagent only reports. Writing to `wiki/`, revising the statement, changes on disk — all through the main thread and with the human's confirmation ("Discipline", §4). Independent review widens the view; it does not get write access.
