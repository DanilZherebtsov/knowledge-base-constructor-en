---
id: 0005
title: "Two-part reply style (.claude/output-styles, outputStyle, hooks) and the whole move to base@49"
adr: adr-0061
impact: structural             # a new file in .claude/ and a new key in someone else's settings.json — not a file swap
applies-to: any                # any assembled instance of any class
requires:                      # input state (from the build fingerprint)
  base: "48"
produces:                      # output state
  base: 49
---

# Migration 0005 — the two-part reply style and the whole move to base@49

> **Executed by the agent INSIDE the instance's live repository** with the fingerprint `base@48`. This is the **only path** from 48 to 49: any other blocks of the `## base@49` changelog entry (if they appear) are carried by this migration too — step 6. Don't apply base@49 by its headings: maintenance compares version numbers, and a version taken half-way will never catch up with the other half. The source is the public mirror (`constructor/base/`). Knowledge — ADR-0061.

## What changes, and the main invariant

1. **`.claude/`** — a new file `output-styles/plain.md` (the reply style: on top what to decide now, below the line "The rest — no decision needed now:" — the rest); the line `"outputStyle": "plain"` in `settings.json` (or, by the human's choice, in `settings.local.json`); new versions of `hooks/consent_nudge.py` (rewriting the reply on the single word "simpler") and `hooks/judge.py` (the question to the human is looked for in the top part of the reply).
2. **Prose** — `CLAUDE.md` (the "In the human's language" and "Simpler" paragraphs, "Close every request", the understanding check, the `.claude/` tree line, "Request resolved"), `methodology/human-language.md`, `methodology/lint.md`, `methodology/bootstrap.md`; `HELP.md` — with the human's "yes" (step 9).

> **Migration invariant.** Only the standard layer changes: `.claude/` (other hooks and keys — byte for byte), `CLAUDE.md`, `methodology/*.md`, `HELP.md` (with "yes"), the fingerprint, one line in `wiki/log.md`; with "only for me" — also `.claude/settings.local.json` and, if needed, `.git/info/exclude` (neither goes into a commit). Wiki page bodies, `raw/`, `STATE.md`, working layers — byte for byte. **Every step is idempotent:** what is already done — skip; an interrupted migration can be re-run.

## Step 1. Input state

The fingerprint (line 3 of `CLAUDE.md`) is `base@48`; otherwise first catch up the feed to 48. Then:
- **Environment.** Not Claude Code (Claude Desktop, Cowork) — do steps 2 and 5–10; don't touch the `outputStyle` line or the hooks (they don't load there). Log: the repository isn't shared — `## [YYYY-MM-DD] setup | style: blocked by the environment` (the "In the human's language" rule will read the style file itself); shared — first ask "turn the new form of replies on for the whole team?" (it can't be turned on only for yourself here — say so): yes — `… | style: blocked by the environment`, no — `… | style: not adding`, no answer — `… | style: awaiting an answer`.
- `.claude/settings.json` exists → branch B; not (the human removed the folder) → branch A.
- **`outputStyle` is already set somewhere:**
  - in `.claude/settings.local.json`, another value — the human's choice: say in one sentence that the project style won't turn on for them; don't touch the keys;
  - in `~/.claude/settings.json` (a personal style X) — say "your style X will be replaced by the project's style in this project" and ask; "keep mine" — in a shared repository write `"outputStyle": "X"` into their `.claude/settings.local.json` (not committed, as in step 3) and leave the shared log alone; in their own (one author) — don't write the line, log `… | style: not adding`;
  - in the project `settings.json` it is already `plain`, but the human never chose it (brought in by migration 0003 or by assembly) — in a shared repository ask the step 3 question anyway.
- **Shared repository:** `git log --format=%ae | sort -u` shows more than one author (noreply addresses not counted); treat a shallow clone as shared.

## Step 2. The style file

Copy `base/.claude/output-styles/plain.md` from the mirror → `.claude/output-styles/plain.md`. An identical file is already there — skip; a different text is there (the human edited it, or it is their own style under that name) — show it and ask, never overwrite silently. The file goes in any environment. The copy didn't go through (the environment's guard) — by the "Writing hooks…" section of `methodology/lint.md`; without the file, don't do step 3.

## Step 3. The `outputStyle` line (Claude Code only)

Only if `.claude/output-styles/plain.md` is in place (step 2).

- **Shared repository — ask first**, "for the whole team or only for you", with the cost: for everyone — the line goes into the shared `settings.json`, and everyone's personal style in this project is overridden; only for you — the line goes into `.claude/settings.local.json` of the folder this chat is open in (not the working copy where the edit is being made), and works only on this computer. No answer — don't write the line, log `… | style: awaiting an answer`.
- **Branch A** (no `settings.json`) — create `.claude/settings.json` with only `{"outputStyle": "plain"}`; don't restore the hooks without asking — the human removed them.
- **Branch B** — add the key at the top level, **without overwriting the file** and without touching other keys. The project file already has another value — ask.
- **"Only for you"** — the key goes into `.claude/settings.local.json` (merge with what is there, e.g. `permissions`); `git check-ignore .claude/settings.local.json` — not ignored → add a line to `.git/info/exclude`, don't touch the shared `.gitignore`; log `## [YYYY-MM-DD] setup | style: only for me`. If `plain.md` isn't in this folder yet (the edit is in another working copy and arrives after the merge) — write the key in step 10, once the file is there.
- **The environment refuses** — `methodology/lint.md`, "Writing hooks when automatic mode blocks it", with the style's literals: an attempt → the question "add the checks" → a retry → a request to switch the mode → a retry; didn't work — `… | style: blocked by the environment`; the human refused — `… | style: not adding`. The line wasn't written — say in one sentence: I will read the style file myself by the "In the human's language" rule, without the reminder before every reply.

## Step 4. Hooks (Claude Code only)

- `.claude/hooks/consent_nudge.py`: identical to the mirror's file — skip. `git hash-object` = `ff883ba8c3c3e472dfae4c62fd44ad1a13db5074` (the base@43–48 version) and `settings.json` has its `UserPromptSubmit` entry — replace it from the mirror, `chmod +x`. No file or no entry (the human removed it) — don't install and don't ask. A different hash — the file was edited: show the differences from the mirror and ask. Without git — the old version is the one without a `[plain]` line and with no other differences from the mirror's previous file.
- `.claude/hooks/judge.py`: the same; the previous version is `f33169cd8e2877389ad0f3b79dfb5947093f3d54`, the entry is `Stop`; without git — the old file has no `def asks_human`.
- The environment refuses — the old files stay (they work as before), say so in one sentence; no literal needed.

## Step 5. Prose

By headings, keeping the project's own additions:
- `CLAUDE.md`: the "In the human's language" block (the paragraph, the four questions and the "Don't throw precision away" paragraph) → the "In the human's language" and "Simpler" paragraphs from the mirror; "Close every request" — the sentence about several asks; the understanding gate — item (3) and the sentence "The form — the reply style…" after (4); the `.claude/` tree line; "Request resolved" — where the offer to record goes ("the last line of the top part of the reply"), moving it when the top part has a question, "ok" in "Consent", "Reply reviewer" (a question in the top part; service notes), "Long work".
- `methodology/human-language.md`, `methodology/lint.md`, `methodology/bootstrap.md` — the mirror's file if the project didn't change them; changed — by meaning, carrying over the differences between the new and the previous mirror version.

## Step 6. Other parts of base@49

Other blocks of the `## base@49` changelog entry, if any — by their "How to adopt".

## Step 7. The project's own rules about message form

Grep `CLAUDE.md` outside the standard blocks, `.claude/skills/` and `roles/` for `Details|first line of the reply|at the end of the message|last line|In the human's language|four questions` (in a Russian-language project — `Подробност|Первая строка ответа|в конце сообщения|последней строкой|На языке человека|четыре вопрос`); in `roles/` — also the "Tone and format" sections in full. To the human — as a list, with a proposed replacement ("in the bottom part of the message", "as the first sentence", etc.); don't edit without a "yes".

## Step 8. Check

- the diff — only the standard layer; `settings.local.json` and `.git/info/exclude` (with "only for me") — not committed;
- `.claude/settings.json` (and `settings.local.json`, if touched) — valid JSON;
- the "simpler" hook — only if it was replaced in step 4 (otherwise skip this item and name the reason): `echo '{"prompt":"<ide_opened_file>x</ide_opened_file>\nsimpler"}' | python3 .claude/hooks/consent_nudge.py` — two lines, the second starts with `[plain]`; the same with `{"prompt":"Simpler!"}` and `{"prompt":" проще. "}`; with `{"prompt":"that would be simpler"}`, `{"prompt":"/simpler"}` and empty input — a single `[consent check]` line; exit code 0.

## Step 9. `HELP.md` — one question to the human

Show and write in with their "yes" — in the text of the mirror's `HELP.md`: the "How I reply" section (before "A new chat per task"), the edit of the "add the checks" item in "Safety", and the line "My answer is unclear… I misread the task…" in "When something goes wrong".

## Step 10. Fingerprint

`base@48` → `base@49` in line 3 of `CLAUDE.md`. To the human: the new form of replies — from a new chat; a project under git with a separate knowledge branch — after the merge and the update of the folder chats are opened in (the deferred "only for me" line goes there too, if it was deferred).

## Rollback

Delete the `outputStyle` line and the `.claude/output-styles/plain.md` file. The "In the human's language" paragraph stays: the content rules work without the style.
