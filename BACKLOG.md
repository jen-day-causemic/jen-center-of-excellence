# Backlog

Ideas and designs to build later. Newest first. Move an item out when it's built or dropped.

## Other open design items

- **Session-start sync in every client repo:** only SBGP has the hook that pulls the latest `main` and flags open branches at session start (`.claude/hooks/session-start-sync.sh`). Roll it out to the other client repos. (11/6/26)
- **Synced Drive folders for client data:** set up Google Drive for desktop so Claude works on client data files in a synced Drive folder instead of a local-only folder, then update the build-folder rules in each client repo's `CLAUDE.md`. (11/6/26)
- **session-close outside the CoE:** the skill lives only in this repo's `.claude/skills`, so client-folder sessions can't invoke `/session-close` (seen in a BGCP session, 10/28/26). Decide where to install it so client sessions see it without it loading in Ascendably sessions. Until then, Claude runs it by reading `skills/session-close/SKILL.md`.
- **Installed skill copies:** `.claude/skills/` holds copies of `skills/`, untracked. Decide whether to commit them, or install them with a small script so they can't drift from the master copies.
- **Lessons inbox:** switch session-close Step 3 to writing new files in `Knowledge/inbox/` (client) or `knowledge/inbox/` (CoE) instead of proposing edits to existing docs, and decide who curates the inbox and how often. *(Proposed, not yet decided.)*
- **Session logs:** consider retiring them once Asana updates reliably carry "what happened and what's next."
- **Client repo "Key links" section:** add to each client repo's `CLAUDE.md` (tracker, Drive folder, project notes, PM workbook, org aliases, key client roles).
- **Firm CLAUDE.md:** make it user-neutral, like session-close.
