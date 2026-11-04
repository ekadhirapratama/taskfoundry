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
Locations come from the root `CLAUDE.md` (the project's top-level one; in a single-repo project, the repo's own). In a session with nobody to answer, write anything you would ask under "needs confirmation" instead of asking.

1. **Use today's sync file if there is one**: when the root `CLAUDE.md` records a mastersheet sync location and it holds a file dated today, use it instead of pulling the mastersheet again.
2. **Otherwise read the mastersheet, read-only**: source varies per project (Google Sheet / Jira / markdown file). If unknown, ask the user once and record it in the root `CLAUDE.md`.
3. **Add the git/PR delta per active repo**: `git log --oneline -10` and `gh pr list --author @me` (or the repo's own convention) to see what actually shipped or is in review since then.
4. **Check MOM entries, issues, and open plans** at the locations in the root `CLAUDE.md`. If no MOM location is recorded, ask the user whether a recent meeting needs accounting for — don't assume a file exists.
5. **Reconcile**: classify each task as done / on-going / todo. Flag mismatches (e.g., committed but mastersheet still shows todo) instead of silently picking one source as truth.
6. **Reply in chat, 25 lines at most, with no file**: a status list ready to report in the daily meeting, then mismatches and anything needing confirmation.

## Common Mistakes
- Trusting the mastersheet or sync file without checking git/PR reality — they drift when updates lag behind actual commits.
- Assuming the same mastersheet type (sheet/Jira/md) across all repos — it's per-project, ask if unrecorded.
- Writing to the mastersheet — this skill only reads it.
