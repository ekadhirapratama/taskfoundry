---
name: mom-init
description: Use right after a meeting ends and you need to log decisions and new tasks before they're forgotten, so they're findable during the next planning session.
---

# MOM Init

## Overview
Turn a just-finished meeting into a dated, consistently-formatted MOM entry — at a location `/taskfoundry:summary` and `/taskfoundry:plan` both know how to find, instead of relying on notes that only live in someone's memory or a chat scrollback.

## When to Use
- Right after any meeting ends, any time of day — this is not part of the morning/`summary`→`plan`→`develop` rhythm, it's triggered by the meeting itself.
- Not for logging tasks that didn't come from a meeting — a direct request or mastersheet item goes straight into `/taskfoundry:plan`'s `Source` field instead.

## Process
1. **Find the MOM location**: check the target repo's `CLAUDE.md` for an existing convention. If none is recorded (first time using `/taskfoundry:mom-init` in this repo), ask the user once where MOM entries should live (e.g. `docs/mom/`), then record the answer in `CLAUDE.md` so it isn't asked again.
2. **Get the meeting content**: if the user hasn't already provided it in the conversation, ask for the meeting title, key decisions, and any new tasks that came out of it.
3. **Write the entry** using `references/template-mom.md`, filled in with what the user provided, saved as a dated file at the recorded location (e.g. `docs/mom/YYYY-MM-DD-<topic>.md`).
4. **Confirm the file path back to the user.**

## Common Mistakes
- Guessing a MOM location instead of checking `CLAUDE.md` first or asking once.
- Skipping the `New Tasks` section — this is what `/taskfoundry:plan` reads to catch tasks that never made it into a mastersheet.
- Writing a MOM entry for something that isn't a meeting output — direct requests belong in a plan doc's `Source` field, not a MOM entry.
