# How to work with me — project guide

A short cheat sheet: how to set tasks, what I can do, what not to do. To pull it up again anytime, type `guide` (or `help`, `manual`).

## How to set a task

The best results come from a task in four strokes: **context → goal → constraints → output format.**

- Weak: "Sort out the folder of contracts."
- Strong: "The `Contracts/2025` folder has contract PDFs. Make a table: counterparty, tax ID, amount, term. If a field isn't found, leave it blank and flag it 'check'. Save as `register.xlsx`, don't touch the originals."

The more precise the input, the less guessing on the output.

## Techniques that work

- **"Ask me 1–2 clarifying questions before you start"** — cuts off a junk result.
- **"Cite the source for every fact"** — cures invention: when I'm required to point to a place in the document, I don't make things up.
- **"Here's a sample of the format I want"** — I'll match your template.
- **"Check the project memory before a serious task"** — I do this on my own, but on an important task you can nudge me for reliability: I'll re-read what's accumulated instead of starting from scratch.
- **"Remember this: …"** — I turn a decision or fact into a project-memory entry with a date and a source. That's how the base grows as you work.
- **A large batch of files:** "process the first 10, show me the result" → then "keep going the same way".

## A new chat per task

Don't run everything in one chat. The longer the conversation, the more old context I'm holding, and the easier I get confused. Task changed — start a new chat.

Nothing is lost: the project memory (the `wiki/` folder and the project rules) is visible from **every** chat. So splitting into short chats is safe — each one stays precise. A chat has grown too long — say "summarize this into a file", start a new chat, and feed the summary in.

If a chat does hit the limit anyway, I compress the earlier conversation into a short summary and carry on from that — with less detail in it. For that case, right after the compaction I re-read what's written down (what's in progress, where we stopped) and continue from the files rather than from my memory of the conversation. That's a safety net, not a replacement: short chats are still more precise.

## Where things live

- **Project memory (`wiki/`)** — decisions, facts, counterparties, with dates and sources. Permanent, visible from any chat.
- **Project rules (`CLAUDE.md`)** — how I behave in this project specifically; I read them at the start of every chat.
- **`STATE.md`** — what's in progress now and what's next (plans, not facts).
- **Chat context** — I hold it during the conversation. Whatever matters I save into project memory as we go — and I'll offer to keep what's worth keeping; you can also just say "remember this". That's why a new chat loses nothing: what's saved is visible from any chat, and only the back-and-forth that was never written down goes away.

## Safety

- By default I work with confirmation on each step — so you have time to stop both a mistake and a harmful instruction hidden in someone else's file or on a website.
- **Only on an explicit "yes":** payments, sending emails, permanent deletion, handing files to third parties.
- Files and sites from outside (clients, the internet) — keep the confirm-each-step mode: they may carry a hidden command aimed at me (prompt injection).
- **Before a serious step I put myself through a review.** Before doing something that is expensive to undo (money, a signature, sending something outward, an irreversible edit), I run the task past several independent checks — each one looking for what's wrong with the plan, not praising it. In my reply I'll show what they found and **where the checks disagreed with each other**: a disagreement is the thing worth your eyes. Small stuff is not affected. Don't want it on a particular task — say "no review, just do it"; want it stricter — say "check this thoroughly".

## Maintenance — once a week

Say "run maintenance" — I'll check the integrity of the project memory and pull in updates without breaking your entries. A good habit is once a week.

## When something goes wrong

- **Stuck** → stop me, say "tell me briefly what you did" — and give a new direction.
- **Gave you the wrong thing** → don't start over: "this doesn't fit because X — redo Y".
- **Looks made up** → "re-check every number against the sources, flag where you didn't find it".
- **Can't see a file** → check the folder is connected; give the exact path if needed.
- **I misread you, or my answer is unclear** → ask me to explain in plain words, or rephrase the task.

## What I can do in this project

<<SLOT HELP-OPS: the list of ACTUALLY assembled operations and flows — filled at assembly ([ASSEMBLY.md](../ASSEMBLY.md) step 9), the same "only what's assembled" principle as the handoff. Phrase it in human language, lead with an action phrase, no internal jargon.
Base set (present in every project): "**process this** / remember this" — I break the material down into project memory; "**what do we know about X** / compare A and B" — I answer from memory with links to sources; "**run maintenance**"; "**create role <name>** / work as role <name>" — a helper for a function (lawyer, devops, fact-checker — whatever you need), you can drop materials for the role into `input/` in advance.
Class-specific (write in ONLY if assembled): the class's unit-of-work flow in its own phrase ("take a task" / "answer a question" / "make a decision"); upcoming and overdue commitments from the calendar; working with your own code ("make a script/site/bot"; if you drop in code that already exists, I'll go through it and describe it into the project's memory first, and only then take on tasks against it); reverse search by sources and the evidence map — only with the claim graph; work on visual things with the `design` mechanic — "**make a presentation / landing page / cover**" (I'll first check who it's for and why, show you a few directions to choose from, then show you the built result; I keep the style consistent myself).>>
