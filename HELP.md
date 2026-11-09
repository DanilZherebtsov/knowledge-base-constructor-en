# How to work with me — project guide

A short cheat sheet: how to set tasks, what I can do, what not to do. To pull it up again anytime, type `help` (or `guide`, `manual`).

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
- **"Data from a PDF or a scan — reconcile the total against the source"** — a PDF or a scan holds no table as such, only how the page looks, and I rebuild the table from that look. It most often goes wrong at a page break: a row that starts at the bottom of one sheet continues on the next. I reconcile the seams and the total (number of rows, sum of a column) against the source myself and show you what did not add up; on data that matters, remind me with this phrase to be safe.
- **"Show me the check catches an error"** — a check I wrote (a reconciliation, a count, a test) can miss errors and still answer "it all reconciles". So when a result rests on such a check, I test it first: I deliberately corrupt a copy of the data where an error is easiest to miss (the last row, a page seam) and see whether the check notices. I don't touch the original. If all is well, I don't report it separately; I'll tell you if I couldn't test it, or if the check missed the planted error while a result you've already seen rested on it. Want to watch me test it yourself — say this phrase. The testing shows the check catches that kind of error, not that it catches everything.
- **"Remember this: …"** — I turn a decision or fact into a project-memory entry with a date and a source. That's how the base grows as you work.
- **A large batch of files:** "process the first 10, show me the result" → then "keep going the same way".

## How I reply

On top — what came out and what needs deciding now. Below the line "The rest — no decision needed now" — what I decided myself, what I did that can't be undone (sent, deleted), and what the numbers rest on: look there if the top says "decided myself" or you want to check me. Too long or unclear — type the single word **simpler**. Want a different form — say so in words, and I'll write that way until the end of the chat.

## A new chat per task

Don't run everything in one chat. The longer the conversation, the more old context I'm holding, and the easier I get confused. Task changed — start a new chat.

Nothing is lost: the project memory (the `wiki/` folder and the project rules) is visible from **every** chat. So splitting into short chats is safe — each one stays precise. A chat has grown too long, or you want to continue part of the work separately — say "hand this off to a new chat". I'll write down where we stopped — in the task's file or in a separate note — and give you a phrase: paste it first into the new chat. Anything important we haven't saved to project memory yet, I'll mark there too and offer to save — in the new chat or at maintenance.

If a chat does hit the limit anyway, I compress the earlier conversation into a short summary and carry on from that — with less detail in it. For that case, right after the compaction I re-read what's written down (what's in progress, where we stopped) and continue from the files rather than from my memory of the conversation. That's a safety net, not a replacement: short chats are still more precise.

## Where things live

- **Project memory (`wiki/`)** — decisions, facts, counterparties, with dates and sources. Permanent, visible from any chat.
- **Project rules (`CLAUDE.md`)** — how I behave in this project specifically; I read them at the start of every chat.
- **`STATE.md`** — what's in progress now and what's next (plans, not facts).
- **`BACKLOG.md`** — things put off for later; it appears once there is something to put off. I don't read it at the start of a chat; I open it when we're choosing the next piece of work or you ask about longer-term plans.
- **Project documents (`output/`)** — my results and your documents. I name where I put each one in my reply. Want to keep some documents in a folder of their own — say "keep contracts in `output/contracts`": from then on I put them there myself. Files that haven't changed for a month and aren't in your folders I move to `output/archive/` during maintenance — look there for anything that seems missing.
- **My notes folder (`tmp/`)** — what I write for myself so the work can continue: records of how a long job is going, handoffs to a new chat. Don't clean it out yourself: a note may hold something important that isn't saved yet. At maintenance I'll name what has been left lying around, and I remove it only on your "yes".
- **Chat context** — I hold it during the conversation. If we solved your question and along the way learned something that will be needed again (your decision, a new fact, the cause of a failure, your correction), I'll offer in one line to save it to project memory (the `wiki/` folder); it's your call. I may also miss something: if it matters, say "remember this", and if I'm saving without your "yes", stop me. What's saved is visible from any chat; only what was never written down goes away with the chat.

## Safety

- It works best in the mode where I ask permission — so you have time to stop both a mistake and a harmful instruction hidden in someone else's file or on a website.
- **Only on an explicit "yes":** payments, sending emails, permanent deletion, handing files to third parties.
- **Don't type passwords or credentials into the chat.** Need me to get into a server, a router, an admin panel, or need a key for some service — I'll tell you where to put it so I can use it without seeing it in our conversation (usually a couple of commands, a minute of work). Already sent it — we treat it as leaked: better to change it right away, the conversation is stored in full.
- Files and sites from outside (clients, the internet) — keep the mode where I ask permission: they may carry a hidden command aimed at me. When I add a document to project memory and find such a command, I don't carry it out — I show it to you; but I can also miss one.
- **I ask you to answer "add the checks"** — this happens during an update, maintenance, or when you yourself asked to bring one of my checks back: it's permission to write my automatic checks (for example, a reminder to offer saving to memory) or the form of my replies into the project settings. I'll name exactly what I'm writing and what it costs. Such a request at any other time or about anything else — answer "don't add them" and ask where it came from: that's how a harmful command from someone else's file could act.
- **Before a serious step I put myself through a review.** Before doing something that is expensive to undo (money, a signature, sending something outward, an irreversible edit), I hand the plan to several separate reviewers: each looks for what's wrong with the plan and doesn't see what the others found. In my reply I'll show what they found and **where the reviewers disagreed**: a disagreement is the thing worth your eyes. Small stuff is not affected. Don't want it on a particular task — say "no review, just do it"; want it stricter — say "check this thoroughly". **Sometimes the app won't let me launch the reviewers** — then I review it myself, and the review comes out weaker: there's no one to disagree with. I'll say so plainly rather than pass it off as a full review; and "check this thoroughly" is something I won't be able to deliver then — I'll say that too. A step that is expensive to undo I won't settle alone — I'll stop and ask you.

## Maintenance — once a week

Say "run maintenance" — I'll check the integrity of the project memory and pull in updates without touching your entries in the project memory. A good habit is once a week. If the entries in the project memory haven't changed since the last maintenance, I won't re-check them — I'll look at the plan, the files and the updates. And if maintenance already ran today and nothing has changed since, I'll only check for updates; need it again — say "run maintenance anyway".

I'll also tidy the documents folder: files that haven't changed for a month and aren't in your folders I'll move to `output/archive/` — I delete nothing, and it can be undone. The first time I'll show you the list and ask; after that I move them myself — if you'd rather I didn't, say "don't move" (changed your mind — "move again"). About folders where nothing has changed for a month, I'll ask. From the work plan (`STATE.md`) I'll remove the lines about finished work older than a week and keep no more than the ten latest — I'll show you what I removed. In my notes folder I'll name what has been lying around and remove it only on your "yes".

## When something goes wrong

- **Stuck** → stop me, say "tell me briefly what you did" — and give a new direction.
- **Gave you the wrong thing** → don't start over: "this doesn't fit because X — redo Y".
- **Looks made up** → "re-check every number against the sources, flag where you didn't find it".
- **I'm doing more than needed** — re-checking what hasn't changed, stretching the work over hours or days → say "this isn't needed for the task". Before any check beyond the task I look at what has changed in what it checks; a needed but lengthy one I first offer to you with its price. Want everything re-checked in full — just say so.
- **Can't see a file** → tell me which folder it's in, or put it in the project folder (for example, in `input/`).
- **My answer is unclear** → type "simpler". **I misread the task** → rephrase it.
- **No idea what I'm even talking about** → say "show me where I wrote that". I have to either point at your own message containing the word, or rewrite without it.
- **I ask you to switch modes** → this happens when, in automatic mode, the app itself turns down my permission requests (to delete a file, to change project settings) without showing them to you. Switch to the mode where I ask permission — I'll tell you where. You can switch back afterwards.
- **The `_to_delete` folder** — things you allowed me to remove but I couldn't delete; drag it to the Trash (Recycle Bin on Windows).
- **An unfamiliar line shows up in the window (say, with the word "error"), or my replies got noticeably slower** → ask me what it is and paste the line here: I'll explain what I know and tell you whether it can be removed.

## What I can do in this project

- "**process this** / remember this" — I sort the material into the project memory.
- "**what do we know about X** / compare A and B" — I answer from memory with links to sources.
- "**run maintenance**" — a check of the project memory plus updates, once a week.
- "**help me choose**" — a supplier, a location, a price, a hire: I gather the options with numbers and sources, and after you choose I record what was decided and why.
- **Due dates and commitments** — rent, taxes, renewals: I keep them in the work plan and remind you of the near and overdue ones.
- "**make a script / site / bot**" — I write our own code: first we briefly agree on what we're doing, then the code and the check. A task we'll come back to I keep as a separate document in `specs/`. You dropped in ready-made code — I first go through it and describe it in the project memory, and only then take on tasks for it.
- "**make a presentation / landing page / cover**" — I first ask who it's for and why, show a few directions to choose from, then show what I've built; I keep the style consistent myself.
- "**create role <name>** / work as role <name>" — a helper for a function (lawyer, devops, fact-checker — whatever you need); you can drop materials for the role into `input/` beforehand. For a site or a bot there are ready-made "developer" and "release manager" samples — on your request.
