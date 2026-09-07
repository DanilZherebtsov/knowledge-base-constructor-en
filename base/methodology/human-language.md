# In the human's language — worked examples

Read on trigger, not in the background. There is exactly one trigger, and it is external: **the human said they didn't understand, or asked you to rewrite.** Then come here and rewrite by this file. The rule itself (four questions asked of a finished message) lives in `CLAUDE.md` and works without this file; what follows is examples and analysis, not a second copy of the rule.

---

## A ✗ / ✓ pair in full

An example from a construction-estimates project. Both texts describe the same event; the human did not understand the first one and asked for a rewrite.

**✗ As it was written:**

> Checked two blockers by execution — one confirmed and it reverses what I told you an hour ago, the other turned out to be false. Confirmed: the kerb-stone material will not be priced after the norm change. In -09 the unaccounted stone is specified in cubic metres (1.6 m³ per 100 m), while the representatives of its tech group 49.02.002 in the price base are per piece (05.2.03.04-0007 300.49 ₽/pc). The engine deliberately refuses to convert across different units (engine.py:508-534, guard PRICING-UNIT-GUARD-001) and emits a visible row with a zero. A recalculation is physically possible, but the engine has nowhere to get the volume of one piece — that is K8/K9, not K6.

**✓ As it should read:**

> **What I told you an hour ago was wrong.** I said the material would be priced automatically — it will not: the quantity will appear, the money will not.
>
> The norm Alyona named specifies stone consumption in cubic metres, while the price book sells it by the piece. The program deliberately does not convert one into the other: it does not know how big one piece is. The row will appear in the estimate with a zero and a note saying "cannot supply a price".
>
> **In money:** labour drops by 9,919 ₽, concrete by roughly another 7,000 ₽, and the stone stays at zero as before. The roughly 45,000 gap against Alyona's own figure for the stone is not going anywhere — but now we will state honestly why.
>
> **The task you set** — moving the estimates onto the new norms — is on track: Alyona's rule is confirmed. 18 estimates left.
>
> Recalculating is possible in principle if we supply the size of one piece. Do that now, or leave it as is?

## What actually changed

| Move | ✗ | ✓ |
|---|---|---|
| Retraction goes first | fifth sentence, mid-paragraph | the heading, ahead of everything |
| A cause you can argue with | "guard PRICING-UNIT-GUARD-001" | "it does not know how big one piece is" |
| Consequence in money | not stated at all | 9,919 ₽ + 7,000 ₽ + a 45,000 gap |
| Where the original task stands | not stated | "on track, 18 estimates left" |
| A question for the human | none | "do that now, or leave it?" |
| Identifiers | 8 of them | none |
| The aside "that is K8/K9, not K6" | present, addressed to nobody | deleted |

Note that ✓ is not shorter than ✗. The requirement is not brevity but clarity and completeness. What gets shortened is the language, not the substance of the decision.

## Substitution glossary

Working words that get replaced or dropped in a message to the human:

| Your word | In a message to the human |
|---|---|
| lens, reviewing subagent | reviewer |
| task statement | the plan |
| blocker | what's in the way |
| reconciliation, gate report | list of findings |
| ingest | wrote it into the project memory |
| maintenance (lint) | routine check |
| gate | review |
| mechanic | set of rules |

Identifiers the rule catches: a file, function, folder, branch, or commit name; a line, position, version, or order number; an error, standard, article, or guard code; a table, field, or collection name; the label of an object, site, or run.

## How to translate a cause

One move: **say what the program physically cannot know**, instead of how it is built inside.

- ✗ "the guard against cross-unit conversion fired" → ✓ "it does not know how big one piece is"
- ✗ "the request to the external service timed out" → ✓ "their server did not answer within a minute"
- ✗ "a uniqueness constraint was violated" → ✓ "that record already exists, and there must not be two of them"
- ✗ "no entry in the embeddings cache" → ✓ "we have not read this document yet"

The check: the human can argue with ✓ and cannot argue with ✗. People can only argue with what they can see for themselves.

## Separate messages: retractions and priced choices

**A retraction.** Open with the retraction itself, not with how you discovered it.

> Yesterday I told you X. That was wrong: it is actually Y. Here is what that changes: … What do we do?

**A priced choice.** The price counts as: money, deadline, data loss, irreversibility, someone else's redone work.

> There is a choice here: A or B. A gives …, costs …; B gives …, costs …. I would take A, because …. How do you want it?

Both go out **immediately**, as soon as they become known, and as a **separate message**. Waiting for the end of the work is not allowed: by then the human has already spent an hour on a premise you are retracting.

A retraction and a choice coinciding in one event — one message, retraction as the first sentence, question as the last.

## Turning reviewers' findings into one message

The requirement "every finding gets an explicit outcome" applies to the **file**: the outcome of each one is written into `tmp/<operation>-<date>/`. That satisfies the gate.

What goes to the human is **neither the list nor a link**, but, in your own words, whatever changes their decision:

- a finding that retracts something you said, or creates a priced choice → a separate message, immediately (see above);
- a finding that changes what the human will get → one or two living sentences inside the general message;
- a finding that changes nothing for them (you fixed it yourself and moved on) → one line covering all of those at once, so they still have time to stop you.

A link to the file in place of the content is forbidden: the human decides from the message.

## Edge cases

- **The human writes in technical words themselves** — answer in kind. The rule does not force you to explain what they used first.
- **A word explained once** — goes without a gloss for the rest of that chat.
- **The human is a developer** — they pronounce code identifiers themselves, so those are allowed; the rule is not suspended, it simply lets more words through.
- **After the conversation is compacted** their original words are gone from context. Take them from the run journal (its first line), where they are recorded verbatim. Substituting your own wording from yesterday is not allowed: it is already a translation.
- **Three nested tasks** — one per message, the outermost one still open. An inner task closed while the outer one did not move — say that out loud: the human believes they are getting closer while they are standing still.
- **A subagent reporting to whoever called it** — not bound by this rule; write precisely, with names and numbers. A subagent writing to the human on your behalf — bound by it.
- **A machine format that was asked for** (a table, JSON, a diff) — hand it over as is; the rule covers the prose around it.
