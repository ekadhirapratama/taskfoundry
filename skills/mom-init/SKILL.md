---
name: mom-init
description: Use right after a meeting ends, or when a direct request arrives that won't be worked on yet, to log it before it's forgotten and keep it findable during the next planning session.
---

# MOM Init

## Overview
Turn a just-finished meeting or a direct request into a dated, consistently-formatted file — at a location `/taskfoundry:summary` and `/taskfoundry:plan` both know how to find, instead of relying on notes that only live in someone's memory or a chat scrollback.

| Mode | Use for | Template | Default path | Max lines |
|------|---------|----------|--------------|-----------|
| default | One meeting | `references/template-mom.md` | `docs/taskfoundry/mom/YYYY-MM-DD-<topic>.md` | 60 |
| `issue` (`/taskfoundry:mom-init issue`) | One decision or direct request | `references/template-issue.md` | `docs/issues/YYYY-MM-DD-<topic>.md` | 25 |

## When to Use
- Right after any meeting ends, any time of day — this is not part of the morning/`summary`→`plan`→`develop` rhythm, it's triggered by the meeting itself.
- `issue` mode: a direct request or decision that isn't being worked on yet. If work starts right away, it goes straight into `/taskfoundry:plan`'s `Source` field instead.

## Process
1. **Find the location**: check the root `CLAUDE.md` (the project's top-level one; in a single-repo project, the repo's own) for this mode's location. If none is recorded, propose the default path above and ask the user to confirm it or give a different one — don't ask an open-ended "where should this live" question, and don't apply the default silently. Record the answer in `CLAUDE.md` so it isn't asked again.
2. **Get the content**: if the user hasn't already provided it in the conversation, ask for the meeting title, key decisions, and new tasks — or, in `issue` mode, who asked and what they asked for.
3. **Write one file** from this mode's template, one topic per file, within its line limit. Never overwrite an existing file; give the new one a different topic slug.
4. **Confirm the file path back to the user.**

## Common Mistakes
- Guessing a location instead of checking `CLAUDE.md` first, proposing the default, and confirming with the user.
- Skipping the `New Tasks` section — this is what `/taskfoundry:plan` reads to catch tasks that never made it into a mastersheet.
- Creating a remote issue from `issue` mode — it writes a local file only. A remote issue needs the user's approval, per issue.
