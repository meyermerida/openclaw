# CLAUDE.md

Guidance for Claude Code (and any AI assistant) working in this repository.

## What This Repository Is

This is **not a software codebase**. There is no source code, no build system, no
tests, no dependencies. It is an **OpenClaw agent workspace** — the persistent home
folder of a personal AI assistant named **Ron**, owned by **Moisés Reyna ("Moy")**.

The "code" here is Markdown: identity, memory, conventions, and local notes. Git is
used as a backup/versioning mechanism for that state, not as a development pipeline.

Two distinct roles can be in play, and they must not be confused:

| Role | Who | What they do |
|------|-----|--------------|
| **The agent** | Ron, at runtime | Reads `SOUL.md` / `USER.md` / memory, talks to Moy in Spanish, writes memory files |
| **The maintainer** | Claude Code, in a dev session | Edits and documents these files as artifacts, commits, pushes |

When invoked as Claude Code on this repo, you are the **maintainer**. Do not adopt
Ron's persona, do not answer as Ron, and do not write into Ron's memory as if you
had lived the sessions. Edit the files; don't roleplay them.

## Layout

```
.
├── AGENTS.md                     # Ron's operating manual — the largest, most authoritative file
├── SOUL.md                       # Personality and values
├── IDENTITY.md                   # Name, creature, vibe, emoji, language (Spanish)
├── USER.md                       # Moy: name, pronouns (él), timezone America/Merida, preferences
├── TOOLS.md                      # Environment-specific notes (cameras, SSH, TTS voices) — template only so far
├── HEARTBEAT.md                  # Periodic-check checklist; intentionally empty to skip heartbeat API calls
├── BOOTSTRAP.md                  # First-run script — meant to be deleted once identity is established
├── .openclaw/workspace-state.json# Runtime state (schema version, bootstrapSeededAt)
├── .learnings/                   # Structured logs: LEARNINGS.md, ERRORS.md, FEATURE_REQUESTS.md
└── *.jpg, *.jpeg                 # Loose user assets (bouquet.jpeg, meyer_post.jpg, meyer_post2.jpg)
```

### Files referenced but not yet created

`AGENTS.md` describes these as part of the memory system. They do **not** exist yet;
create them only when there is real content to put in them:

- `memory/YYYY-MM-DD.md` — daily raw logs
- `MEMORY.md` — curated long-term memory
- `memory/heartbeat-state.json` — last-check timestamps per channel

### Known drift

`BOOTSTRAP.md` instructs the agent to delete it after identity is settled.
`IDENTITY.md` and `USER.md` are filled in, so bootstrap is complete, yet the file is
still present. Don't delete it unilaterally — flag it and let Moy decide.

## Precedence

`AGENTS.md` is the operating manual and takes precedence on agent behavior. This
file describes the repository for tooling purposes. If they disagree about how Ron
should behave, `AGENTS.md` wins — and the fix is to update `AGENTS.md`, not to
contradict it here.

## Conventions

### Language

- `IDENTITY.md` and `USER.md` are written in **Spanish** — Ron's default language with Moy.
- `AGENTS.md`, `SOUL.md`, `TOOLS.md`, `BOOTSTRAP.md` and `.learnings/*` are the
  upstream OpenClaw templates and remain in **English**.
- Keep each file in the language it is already in. Commit messages have been Spanish
  or the `backup: <date>` form; match the surrounding style.

### Commit style

Existing history uses two shapes:

- `init: primer commit - identidad y configuración de Ron` — meaningful change
- `backup: 2026-03-18 10:03` — automated hourly workspace snapshot

Human/assistant-authored changes should use a descriptive message, not the `backup:`
form. Do not machine-generate `backup:` commits by hand.

### Writing memory

From `AGENTS.md`, non-negotiable:

- **Text > brain.** No "mental notes" — if it should survive a session restart, it
  goes in a file.
- Daily files are raw logs; `MEMORY.md` is distilled and curated.
- `MEMORY.md` loads **only in the main session** (direct chat with Moy). Never in
  Discord, group chats, or any shared context — this is a security boundary.

### `.learnings/` format

`LEARNINGS.md` defines a structured entry format. Follow it exactly when adding entries:

- ID: `[LRN-YYYYMMDD-NNN]` followed by a category
- Categories: `correction` | `insight` | `knowledge_gap` | `best_practice`
- Areas: `frontend` | `backend` | `infra` | `tests` | `docs` | `config`
- Statuses: `pending` | `in_progress` | `resolved` | `wont_fix` | `promoted` | `promoted_to_skill`
- Fields: `**Logged**` (ISO 8601), `**Priority**`, `**Status**`, `**Area**`, then a `### Summary`
- When promoting to a skill, add `**Skill-Path**: skills/skill-name`

`ERRORS.md` (command/tool failures) and `FEATURE_REQUESTS.md` (requested
capabilities) are currently headers only — append below the `---`.

## Runtime Workflows (what Ron does)

Documented here so a maintainer edits them coherently; see `AGENTS.md` for the full text.

**Session startup order:** `SOUL.md` → `USER.md` → today's and yesterday's
`memory/YYYY-MM-DD.md` → `MEMORY.md` (main session only). No permission asked.

**Heartbeat vs cron:**

- *Heartbeat* — batched, drifty checks (~30 min), needs conversational context. Keep
  `HEARTBEAT.md` small: every line costs tokens on every poll. An empty file skips
  the API call entirely, which is the current deliberate state.
- *Cron* — exact timing, isolated from session history, different model/thinking
  level, one-shot reminders, direct-to-channel output.

**Platform formatting:** no Markdown tables on Discord/WhatsApp (use bullets); wrap
Discord links in `<>` to suppress embeds; no headers on WhatsApp — use bold or CAPS.

## Red Lines

Carried from `AGENTS.md` and `SOUL.md`; treat these as hard constraints:

- Never exfiltrate private data.
- No destructive commands without asking. Prefer `trash` over `rm`.
- Ask before anything that leaves the machine — emails, tweets, public posts.
- Reading, organizing, and learning inside this workspace is free; external actions
  are not.
- In group chats Ron is a participant, not Moy's voice or proxy.
- Only Moy gives Ron orders (`IDENTITY.md`, `USER.md`).
- If you change `SOUL.md`, tell the user — the file itself requires it.

## Maintainer Notes

- No package manager, linter, formatter, or test suite exists. There is nothing to
  build and nothing to run. Verification means re-reading the Markdown.
- No `.gitignore`. Binary assets are committed directly; that is the existing
  (if unusual) convention here.
- Loose image files in the root are Moy's assets, not workspace infrastructure.
  Don't reorganize or delete them without asking.
- Keep `HEARTBEAT.md` empty unless Moy explicitly wants periodic checks.
