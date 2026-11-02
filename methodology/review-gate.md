# The independent review gate — before doing the work

One mechanism for every task: **before a nontrivial task is executed, its task statement passes several independent reviews**. "Independent" = a separate subagent with its own context; reviewers **do not see each other's verdicts**; each one's instruction is to look for *what is wrong*, not to confirm what has already been decided.

Read on trigger from `CLAUDE.md` ("How Claude works on tasks"). For our own code, [software-engineering.md](software-engineering.md) builds its execution cycle and the gate's second half on top of this one (a re-review after the code is written, correctness through an actual run).

## What the gate catches, and what it does not

**Catches:** a missed requirement, an unrealistic acceptance criterion, a neighbouring area disturbed, an unnoticed irreversibility, a superfluous step the goal does not require.

**Does not catch divergence from the human's intent.** Reviewers inherit your reading of the task and will be wrong together with you. That is why "confirm understanding" (`CLAUDE.md`) comes **before** the gate, not instead of it: first the human confirms the task was understood correctly, and only then is the statement itself reviewed.

**How well reviewers are grounded depends on the task.** Where there is code, a claim can be checked by running it. Where there is no code (a decision, a contract, a research conclusion), the reviewer leans only on the statement and on `wiki/`, and there is nothing to check the reviewer against. So there the gate's output is **material for the human**, not a verdict of "clean": a widening of the view and support for the decision, not verification. Report it in exactly those words; do not pass it off as verification.

## If you can settle it by doing it, settle it by doing it

Lenses do not replace a run. Where there is a way to **check** (run a test, recompute, compare against the source, ask the counterparty), that is primary — cheaper and more reliable than any discussion. The gate is for what cannot be settled that way, or cannot be settled yet.

Without this rule the gate turns into a debate about something that resolves in ten seconds.

**Test your own check with a planted error first.** A check here is anything that itself returns "no errors" or "it reconciles": a program, a test, a formula, a query, a data-validation rule, the prompt of a grader model (another model returns "pass / fail"). The task's result is accepted on a check you created or changed in what it asserts or catches — or the result is that check's answer itself ("no discrepancies"), or you start relying on an existing check for a different kind of error than the one it was written for (an ordinary regression run of existing tests is not new reliance). Then, before you trust the check's answer, plant an error in its input on purpose and see whether it catches it:

- **Which error.** The one you rely on the check to catch, no smaller than its tolerance if it has one — in the forms it actually takes in this data (look at the data, don't invent), and always in a hard spot: inside a list (`[3, 99]`, not a standalone `[99]`), in the last row, at a page seam — not only in the most visible one. A grader model gets a small set of corrupted and clean examples, not one.
- **What counts.** On the same input without the planted error the check stays quiet, and with it the check fires because of it: a row-level check points at it, a total-level check is off by exactly the planted amount, a test fails on its own assertion, not on a startup or import error. It didn't catch it — fix the check, don't trust it. It caught it — it catches this kind of error, not everything.
- **Where, without breaking anything.** The input is a small local sample in `tmp/<operation>-<date>/`, including the hard spot, with no secrets and no personal data (or anonymized); under git, `tmp/` goes into `.gitignore` before the first write. A total-level reconciliation can use a sample only together with its own control total; without one — a copy of the whole input, same place. The test run's output also goes to `tmp/` (check where the check writes), and the task's result comes from a run on the original. The sample goes out with the normal `tmp/` cleanup after acceptance. Test the project's own code only in a copy and following the procedure in [software-engineering.md](software-engineering.md); if that procedure is missing — don't plant errors in code.
- **What not to test.** First read what the check does when it finds an error: notifies people, moves files, writes to an external system, halts the work — disable that branch for the test with a flag or on a copy of the check in `tmp/`, not by editing the working file; otherwise don't test it. A check that can't run on a local file (a shared spreadsheet, a production database) — don't test it in the external system. A model call the check also makes in its normal runs is not "outside". Installing software for the sake of the test — only with the human's "yes".
- **Order and trace.** The test belongs in the success (acceptance) criteria; decided to write such a check mid-work — add the test to the task statement or the run journal. The step is mandatory (the "The test does not cancel" list in `CLAUDE.md`): it is the check itself being tested, not the data — this is not a repeat run; the "is there something cheaper" question still applies. Once — and again if the format or source of the data it reads has changed. Trace — a line in the task statement, the run journal, or a comment in the check itself: what was planted, where, what the check returned; "catches: <kind>; untested on the rest".
- **To the human.** Success stays silent. Report when testing the check is impossible or didn't work out (why; if you tried — what happened) and which result rests on the untested check. It didn't catch it, and a result the human has already seen rested on it — that retracts something you said: a separate message, right away ("In the human's language" in `CLAUDE.md`). If the environment asks the human for permission to write the copy — one line before that prompt saying you are testing your own check on a corrupted copy and not touching the original; don't ask for permission in the chat.

Does not apply to your own reading of a document, to working searches along the way, or to how a gate reviewer subagent backs up its own remark.

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

- **Correctness, completeness, and the superfluous** — does the statement solve the task as posed; is a requirement missing; are the acceptance criteria realistic — and, if acceptance rests on a new or changed check, is testing it with a planted error among them (section "If you can settle it by doing it"); are there steps the goal does not require (the "Extra work" test in `CLAUDE.md`).
- **Impact on neighbouring areas** — what else in the system or in the business is affected; which obligations, agreements, contracts, invariants break; where the regressions are. Input — the knowledge in `wiki/`. Flags irreversibility and blast radius (see above). **If the wiki is empty on the area affected — report exactly that ("there was nothing to read"), not "no impact".** An empty lens counted as a clean one is the worst outcome a gate can have: it looks passed. What follows is the human's call: build that knowledge first, or proceed with the risk explicitly accepted.
- **Disproof** (strengthened contour only) — the instruction is not "assess" but **disprove**: produce a scenario in which this decision does harm. Found nothing — say so; silence is a result too.

**A request that is exhaustive by its nature** ("all the contracts", "every type of X", "the whole body of regulations") — the statement enumerates the items as an **explicit list**, not as one covering phrase. Otherwise a dropped item is invisible both to the reviewers and to the human.

## Reconciling the findings

**Every finding gets an explicit outcome** — accepted (what exactly changes) or rejected (why). Collapsing three objections into one line, restating an inconvenient one more softly, or quietly dropping it — forbidden. This is a floor rule: it always applies and costs nothing.

**A finding that adds work goes through the "Extra work" test** (`CLAUDE.md`). An accepted one names in its outcome the risk it closes (question (2)); a rejection names which question of the test failed and how; a finding that shows a chain of dependency on what changed is rejected only by proof that the chain does not exist. A rejected finding is written into the statement as a line "not doing this, because…" — otherwise the next round raises it again. Any change to a deadline given to the human — in a separate message to them at once, not as a line in the reconciliation.

Findings reconciled → the statement is revised → the gate **repeats**, until it comes back clean — no findings the goal needs remain.

**The reconciliation is a working document, not a message to the human.** The outcome of every finding is written to a file in `tmp/<operation>-<date>/`; that is where the requirement above is satisfied. Printing that file to the human is not allowed — this is exactly how a wall of text is born, with the important part drowned in it. What goes out is an address to them under the "In the human's language" rule (`CLAUDE.md`): in your own words, whatever changes their decision — and a retraction of something you told them earlier, or a choice with a price, does not wait for the reconciliation: it goes out as a separate message at once. A link to the file in place of the content will not do: the human decides from the message.

## When to stop

**The number of lenses is tied to the surface, not to anxiety.** However many distinct areas genuinely need looking at — that many lenses. A genuinely wide task legitimately gets more; multiplying identical reviewers over the same piece is not quality, it is noise and a queue.

**Not converging in ~two rounds means the statement itself, or the recon under it, is defective.** Fix that, don't add rounds.

**There are no numeric agreement scores or stopping thresholds.** "Agreement 0.8" looks like a measurement and is an invention — "Discipline", §3. What stops the gate is a structural signal (clean / statement defective), not a number.

## The reviewers cannot be launched

There may be no separate reviewers to be had: the tool is absent in this environment, launching them is forbidden by session policy, the call is refused. The gate is then **neither cancelled nor passed in silence** — it is passed weaker, and that is said out loud. A silent pass presented as a passed gate is the same empty lens counted as a clean one: it looks like it happened.

**The refusal must be observable.** Try first, conclude after: "assumed it wasn't allowed" is not grounds — one shared context is cheaper and therefore pulls. The refusal applies to the task at hand and is not inherited onward through the session. A reviewer that did launch and then failed is a failed lens (reported as a gap), not an absent mechanism.

**Irreversible and wide-impact work does not degrade.** On the strengthened contour there is no substitute: a disproof round run inside the shared context is not weakened but imitated, because it disproves its own work and already knows every earlier verdict. The reviewers cannot be launched and the task is irreversible → **stop and ask the human**, rather than pass with something weaker.

**Below the strengthened contour — what remains:**

1. **Doing it first.** Everything that converts into settling it by doing — a run, a recomputation, a check against the source — is converted (see "If you can settle it by doing it, settle it by doing it"). When independence is lost this is the one part of the gate that loses nothing, so its share grows rather than staying put.
2. **The remainder — passes with different instructions**, not the same number of lenses. Copies of one instruction inside a shared context do not create independence: a later pass reads the earlier ones and agrees with them. The variety of instructions is kept; the count is not.
3. **The outcome is material for the human, not a verdict of "clean"** (the same wording as for tasks with no executable check). What gets disclosed is exactly what changed: **the checks ran in one shared context and did not check each other** — in the human's language, without tool names. The line is needed only here: a normal pass says nothing about its own mode, otherwise the marker turns into a ritual and stops being read.

**Reconnaissance is a different trade-off.** A recon pass over unfamiliar material needs not independence but breadth of context, so its honest degradation is a sequential sweep that writes each piece's result to disk as it goes. But **recon counts only by what has landed in `wiki/` as pages** (per module, per contract, per area — with the human's confirmation). Reading done in passing during the gate is not recon, however much of it there was, and it does not clear the empty-lens blocker.

## Reviewers write nothing

A subagent only reports. Writing to `wiki/`, revising the statement, changes on disk — all through the main thread and with the human's confirmation ("Discipline", §4). Independent review widens the view; it does not get write access.

**The pass runs inside the shared context** (section above) — the rule is the same: while the check is running, `wiki/`, the statement and the working files are not touched; edits come after the findings are reconciled. This is about write access, not about scratch: the pass's own journal goes to `tmp/`, as with any long pass. Otherwise a context compaction mid-pass leaves an empty disk and the line "a gate was running" in the summary — and the cheapest move becomes counting it as having happened.
