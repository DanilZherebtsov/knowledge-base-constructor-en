# Credentials and secrets — how the human hands them over safely

The rule kicks in when a task runs into **getting into something**: a server, a VM, a router or another device over SSH, a site's admin panel, a database, a cloud console, a paid API, a mailbox, a payment dashboard. The always-on trigger is in `CLAUDE.md`; the order of steps and the channels are here.

## The order — in one reply, before the first step of the task

1. **Warn before the human starts sending.** In the very first reply where it becomes clear that access is needed: credentials don't go into the chat — the conversation is stored in full (chat history, session files on disk), and it cannot be scrubbed after the fact. Waiting silently for a password is not an option: once it arrives, warning is too late.
2. **Offer ONE channel** — the highest applicable rung of the ladder below — with the actual commands and with what the human does by hand. Not a menu of five options and not a lecture on security: two or three lines, so they can close the question in a minute.
3. **It didn't fit — name the next one** (the device can't do keys, the tool isn't installed). Walk down the ladder rather than starting at its bottom.

## The ladder of channels — top down

**Rung 1 — the secret never reaches Claude at all.** A tool holds the access, and the human authorizes it once, themselves:

- **SSH to a server / VM / device** — a key instead of a password: the human runs `ssh-copy-id <user>@<host>` (or uploads the public key through the device's web interface — the typical path for a router) and adds an alias to `~/.ssh/config`. From then on Claude runs `ssh <alias>` and sees neither the password nor the key.
- **A CLI with its own login** — the human runs `gh auth login`, `aws configure`, `az login`, `gcloud auth login`, `docker login`, `vercel login` once, or puts `~/.pgpass` in place for `psql`. From then on Claude simply calls the command.

This is the best outcome: the secret appears neither in the conversation nor in the project's files.

**Rung 2 — the command itself needs the value** (a token, an API key, a connection string):

- **A `.env` file in the project root.** The order is: **Claude writes the file name into `.gitignore` first** (no `.gitignore` — it creates one), and only then the human creates the file **in their own editor**, without dictating its contents into the chat. The guard line appears **before** a secret is inside — the other way round does not work. The project is not under git — say plainly that there will be no protection from an accidental commit. The file is already tracked by git — `.gitignore` won't switch it off: that needs `git rm --cached <file>`, and if it has already reached history or a remote, the secret counts as leaked (see below).
- **Or a secret manager:** `op read "op://<vault>/<item>/<field>"` (1Password CLI), `security find-generic-password -w -s <name>` (Keychain, macOS), `pass show <name>`.
- Claude uses the value **through the variable name** (`$TOKEN`), never through its contents: it doesn't open the file, doesn't print the value into a reply, and doesn't paste it into a command as text.

**Rung 3 — someone else's web interface** (an admin panel, a bank client, a marketplace dashboard). Claude does not log in there on the human's behalf. Either the human issues a separate API key or a scoped service account — and the task returns to rung 2 — or they take that step themselves while Claude prepares what to paste and where to click.

## Hygiene — whatever the channel

- **The agent's access is separate, not personal:** its own key, its own token, minimum rights (read-only where that suffices), an expiry or a way to revoke it. A personal admin password is the last resort, not the first.
- **No echo.** A secret's value is never printed into a reply, never dumped with `cat`/`echo`, and never lands in a run journal, a commit, `wiki/`, or `STATE.md`. What gets written down is only the **name** of the variable and **where it lives** — enough to pick the task back up a month later.
- **One-off access gets revoked** once the task is done — offer that in the report instead of leaving it hanging.

## A secret did end up in the chat

Treat it as **leaked**, however quickly the message was deleted: the conversation is already recorded, and deleting a message does not rewrite it.

- Say plainly that the access is compromised, and offer to **rotate or revoke** it before the task goes on.
- Rotation is expensive or impossible right now (no access to the panel, the password is shared with the whole household) — name the risk in words and continue **only on an explicit "yes"** from the human. Silently working on with a leaked secret is not allowed.
- Don't move the leaked value into the project's files "so it isn't lost": after rotation it is dead anyway.
