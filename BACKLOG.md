# Backlog

Ideas and designs to build later. Newest first. Move an item out when it's built or dropped.

## Project-close decision and action log (idea, 2026-10-25)

**Problem:** During a project, sessions produce a lot of detail: session logs, inbox lessons, approval tables, Asana updates. That's useful while the work is live, but keeping all of it forever creates clutter with no process for deciding what to retain.

**Idea:** A separate skill, run when a client project closes, that:

- reads the project's working detail (session logs, `Knowledge/`, inbox, Asana history);
- produces one concise **decision and action log**: what was decided, by whom, when, and the key actions taken, without per-session narrative;
- proposes what to keep as lasting reference and what to archive or delete.

**Open questions:**

- Where the final log lives (client repo, Google Drive, both).
- What happens to the working detail afterwards: archive, delete, or keep.
- Whether a lighter version runs at phase ends, not just project close.

## Other open design items

- **session-close outside the CoE:** the skill lives only in this repo's `.claude/skills`, so client-folder sessions can't invoke `/session-close` (seen in a BGCP session, 10/28/26). Decide where to install it so client sessions see it without it loading in Ascendably sessions. Until then, Claude runs it by reading `skills/session-close/SKILL.md`.
- **Installed skill copies:** `.claude/skills/` holds copies of `skills/`, untracked. Decide whether to commit them, or install them with a small script so they can't drift from the master copies.
- **Lessons inbox:** switch session-close Step 3 to writing new files in `Knowledge/inbox/` (client) or `knowledge/inbox/` (CoE) instead of proposing edits to existing docs, and decide who curates the inbox and how often. *(Proposed, not yet decided.)*
- **Session logs:** consider retiring them once Asana updates reliably carry "what happened and what's next."
- **Client repo "Key links" section:** add to each client repo's `CLAUDE.md` (tracker, Drive folder, project notes, PM workbook, org aliases, key client roles).
- **Firm CLAUDE.md:** make it user-neutral, like session-close.
