---
name: summary
description: Use when starting the work day and need to reconcile yesterday's task status against actual git/PR history, mastersheet, and prior meeting notes before today's planning.
---

# Morning Summary

## Overview
Recompute task status (done/ongoing/todo) from ground truth — git/PR activity and the mastersheet — instead of trusting memory or yesterday's assumptions, before reporting in the daily meeting.

## When to Use
- First thing when starting a work session for the day, before `/taskfoundry:plan`.
- Not for mid-day status checks — this is a start-of-day reconciliation.

## Checklist
1. **Summarize recent activity per active repo**: `git log --oneline -10` and `gh pr list --author @me` (or the repo's own convention) to see what actually shipped or is in review.
2. **Check the mastersheet**: source varies per project (Google Sheet / Jira / markdown file). If unknown for this repo, ask the user once and note it — ideally recorded in that repo's `CLAUDE.md` so it isn't asked again next time.
3. **Check prior meeting notes (MOM)**: read the MOM location recorded in this repo's `CLAUDE.md` (set up via `/taskfoundry:mom-init`). If no MOM convention is recorded yet, ask the user directly whether there's anything from a recent meeting to account for — don't assume a MOM file exists.
4. **Reconcile**: compare step 1 (what actually happened) against steps 2-3 (what was supposed to happen) → classify each task as done / on-going / todo. Flag mismatches (e.g., committed but mastersheet still shows todo) instead of silently picking one source as truth.
5. **Produce the meeting summary**: a short status list ready to report in the daily meeting.

## Common Mistakes
- Trusting the mastersheet without checking git/PR reality — it drifts when updates lag behind actual commits.
- Assuming the same mastersheet type (sheet/Jira/md) across all repos — it's per-project, ask if unrecorded.
- Assuming a MOM file exists at a guessed path instead of reading the convention `/taskfoundry:mom-init` recorded, or asking if none was recorded.
