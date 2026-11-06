# 2026-11-05/06: Knowledge governance deck, section-to-repo map, project closeout skill

**Session by:** Jen Day
**Repo / org touched:** jen_center_of_excellence; client repos south-baltimore-gateway-partnership-SBGP, arizona-humane-society, boys-and-girls-club-portland, clean-slate-initiative; Google Slides deck; SBGP Slack channel. No Salesforce org.
**Goal:** Settle what lives in Git vs Google Drive for people and Claude working together, make every session find the right repo, and add an end-of-project cleanup.

## What changed
- `firm-config/causemic-firm-CLAUDE.md`: Repo column in the sidebar-section table, a "Get current with the repo before the task" rule (run the repo's session-start check when the session isn't inside it), Gateway spelling fixed. Installed copy matches. (372ee21)
- Deck [Knowledge Governance w GitHub, Google Drive, and Claude](https://docs.google.com/presentation/d/1z_i5GEuH1R29HXp8ky9CLzXkxhvmLx13whA8ezm2NSQ/edit), 10 slides: what goes where (Operational / Project-Client / Systems), Git vs Drive matrix, Git and Drive roles, data-heavy workflow examples, client Drive folder structure, repo architecture, client repo template, repo visibility and permissions standards, session standards.
- `/project-closeout` skill added to all four client repos: SBGP 02a3fcf and 2930402, AZHS c09e935, BGCP b9d9002, CSI d7aec27. Built from the BACKLOG idea "Project-close decision and action log" (removed from BACKLOG).
- SBGP repo: session-start sync hook. Details in that repo's session log.

## Decisions
- Dividing line: the client repo holds the knowledge and decisions people and Claude work from; Drive holds client data, recordings and client-facing deliverables (decided by: Jen)
- The closeout skill lives in each client repo (decided by: Jen)

## Verified / not verified
- Verified: installed firm config matches its source; slide 2 render checked; all five repos clean and in step with GitHub.
- Not verified: deck not yet reviewed by Addie or the team; synced-Drive workflow untested (Google Drive for desktop not installed on Jen's Mac); `/project-closeout` not yet run end to end.

## Open questions
- Branching (Addie's suggestion) vs the SBGP team's `main`-only practice (owner: Jen; at Addie's Git meeting)
- Add a Systems and Tools folder to the CauseMic CoE in the repo architecture doc, for firm tools (Asana, QuickBooks Time, GitHub, Claude)? Claude's recommendation: platform tools go to the CRM CoE, firm tools to the CauseMic CoE, client-specific setup to the client repo (owner: Jen)

## Next
- Next action: share the deck with Addie before her Git meeting (suggested)
- Pick up from: the deck link above
- Waiting on a person: Addie (repo access review, Git meeting)
