# 2026-11-03: GitHub setup, firm rules, session-close and Asana skills

**Session by:** Jen Day
**Repo / org touched:** jen_center_of_excellence; CauseMic-Hub (client repos created); Asana (one test task, since deleted); Google Doc "CauseMic GitHub Repo Architecture v1"
**Goal:** Design how CauseMic uses GitHub with Claude, keep CauseMic and Ascendably work separate on one Mac, and build the first shared skills.

## What changed
- Mac: CauseMic SSH key served by 1Password (Touch ID), `github-causemic` SSH alias, CauseMic Git identity for CauseMic folders only. Ascendably setup untouched. See runbooks/causemic-ascendably-git-separation.md.
- CauseMic-Hub: client repos created and cloned under ~/VSCODE/Clients/CauseMic/.
- This repo: runbooks (Git separation; team SSH setup guide), skills/session-close, skills/asana, firm-config/causemic-firm-CLAUDE.md, BACKLOG.md. Skills installed in .claude/skills for testing.
- Asana: description format and custom fields tested on a throwaway task (deleted after the test).
- Google Doc: added a "10/22/26 Claude Proposal" tab with a proposed folder structure.

## Decisions
- GitHub org use is limited to client repos for now; no broader architecture yet (decided by: Addie)
- One repo per client, snake/kebab-case full names (decided by: Jen)
- Separate CauseMic and Ascendably by folder and an SSH alias; never modify the Ascendably setup (decided by: Jen)
- CauseMic Claude work runs in Claude Desktop, not the terminal `claude` command, which is signed into Ascendably (decided by: Jen)
- Asana rules live in their own skill; session-close verifies Asana changes already made rather than redoing them (decided by: Jen)
- Asana description updates: core text, divider, then "m/d/yy Name – update", newest first (decided by: Jen)

## Verified / not verified
- Verified: Git identity and SSH separation; clone and push via github-causemic; Asana description format and custom fields
- Not verified: /session-close run as an installed skill in a fresh session; the team SSH guide on another person's Mac

## Open questions
- Should the installed skill copies in .claude/ be committed? (owner: Jen)
- Lessons inbox vs. direct doc edits in session-close (owner: Jen; see BACKLOG.md)

## Next
- Next action: send the SSH guide to Elizabeth and Roger in Slack
- Pick up from: runbooks/causemic-github-ssh-setup-mac.md
- Deliberately left undone: GitHub command-line sign-in for CauseMic (not needed yet; pull requests are opened in the browser)
- Waiting on a person: Elizabeth and Roger to complete the SSH setup
