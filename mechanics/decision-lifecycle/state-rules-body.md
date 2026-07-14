# State-rules body for business (decision-lifecycle)

Inserted by wiring into base `state-rules.md`: replaces the S5 slot region ("## Structure"), refines rule 2b, and **appends the "Commitments calendar" section**. Base rules 1, 3–7 come from base.

## Structure (7 sections)

1. **Snapshot** — a picture of "where we stand": the stage of the business, key numbers with a source. Not a plan — the current picture.
2. **Open tracks** — parallel finite efforts in progress (open a location, launch a product). Each — with its own outcome and a link to the key decisions/comparisons in the wiki. There are usually several tracks, running in parallel.
3. **This week** — what is in flight right now (one or two tasks).
4. **Next (1–2 weeks)** — the near-term backlog, one-liners. Beyond the horizon — into "Open tracks".
5. **Commitments calendar** — recurring and dated commitments (rent, taxes, payroll, renewals, reporting): due date + cadence. Related documents — by link to `entities/`/`decisions/`. Mechanics — below.
6. **Blockers, risks, and decisions waiting on me** — what is in the way (a blocker), what could get in the way (a risk + a plan B), what awaits your word (with a link to `output/drafts/` or `synthesis/`).
7. **Recently completed** — the buffer zone before collapsing.

**Granularity mirrors the horizon of understanding.** Early on, a track is 2–3 lines in broad strokes. As it approaches, it gets broken down. Don't atomize prematurely — that is false precision.

## Rule 2b (moving + commitments)

Once a significant item is completed, Claude moves it from "This week" to "Recently completed". After an ingest that adds a commitment (a lease signed, a legal structure with a quarterly tax chosen, an employee hired) — proposes a line in the "Commitments calendar".

## Commitments calendar — how it is kept

STATE's section 5 is not a task list but a list of **commitments**: things that recur or have a hard external deadline with consequences for missing it (a fine, penalties, a broken contract, a revoked license). A task is done once and closed; a commitment cannot be "completed" — only serviced until the next cycle.

**What goes in.** Rent, taxes and contributions, payroll and regular payouts, renewal of contracts / licenses / domains / insurance, periodic reporting, recurring payments to suppliers.

**Line format:** what — due date and cadence — amount (with a source) — link to the related entity/decision/document.

```text
- Rent (Main St) — monthly on the 5th, $2,400 → [landlord-main-st](../wiki/entities/landlord-main-st.md)
- Quarterly tax — quarterly, next due 2026-07-28
- Domain renewal — annually, by 2027-03-14
```

**How it is replenished.** The human adds entries manually; Claude — via the cascade (rule 2b): an ingest or a decision produced a recurring commitment → proposes a line, taking the due date and amount from the source, never inventing them.

**Reminder threshold.** A commitment within 7 days, or an overdue one, Claude raises in the session-start (silent) check; lint additionally flags near/overdue ones in its report (this is the business DOMAIN-LINT).

**On fulfillment.** A recurring one does not move to "Recently completed" — Claude updates the "next due" to the next cycle. A one-off (a single renewal) is removed from the calendar after fulfillment.

**This is a list, not automation.** The reminders are made by Claude while reading, not by a background process.
