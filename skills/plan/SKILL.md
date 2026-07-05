---
name: plan
description: Use when turning today's new meeting notes or mastersheet items into a written implementation plan, before starting execution.
---

# Daily Plan

## Overview
Turn today's new tasks (from MOM or mastersheet, surfaced by `/taskfoundry:summary`) into a structured plan doc using this project's own template — not an ad-hoc one — so plans stay consistent across every repo.

## When to Use
- After `/taskfoundry:summary`, once new or leftover tasks for today are identified.
- Not for large multi-week features — for those, use full `superpowers:brainstorming` → `superpowers:writing-plans` instead (if the `superpowers` plugin is installed). This skill is for same-day task planning only.

## Process
1. **Identify today's tasks**: from MOM entries (read the location recorded in this repo's `CLAUDE.md` — set up via `/taskfoundry:mom-init`; if nothing is recorded, ask the user directly) or mastersheet items surfaced this morning.
2. **Analyze the relevant repo**: read the code the task touches before writing the plan — don't plan blind.
3. **Clarify ambiguous tasks**: ask one question at a time, don't assume — the same discipline `superpowers:brainstorming` uses for bigger features, applied at daily-task scale.
4. **Write the plan** using `references/template-plan-structure.md` as the structural reference. Every plan MUST start with a `Source` field recording where the task came from: a mastersheet link, ticket ID, or `direct request`. This is what lets `/taskfoundry:develop` fill the PR's `Related` section later without guessing.
5. **Decide where to save the plan doc**:
   - Check whether the target repo already has an established convention — an existing plan-docs directory with prior dated files, or a note in that repo's `CLAUDE.md`.
   - If a convention exists, follow it.
   - If nothing is found (first time using `/taskfoundry:plan` in this repo), propose the default `docs/taskfoundry/plans/YYYY-MM-DD-<topic>.md` and ask the user to confirm it or give a different location — don't ask an open-ended question, and don't apply the default silently. Record whichever answer they give in that repo's `CLAUDE.md` (e.g. `Plan docs go in docs/taskfoundry/plans/YYYY-MM-DD-<topic>.md`) so the question isn't repeated next time.

## Common Mistakes
- Silently applying the default path without confirmation — propose it, then wait for the user's yes or override.
- Skipping the `Source` field — it's the only reason `/taskfoundry:develop` can fill the PR's `Related` section truthfully instead of guessing.
- Writing the plan before reading the affected code — leads to tasks that don't match what the codebase actually needs.
- Treating this as a substitute for full spec-and-plan work on multi-day features — it isn't.
- Guessing a plan-doc location in a repo that's never used `/taskfoundry:plan` before instead of asking once and recording it.
- Assuming a MOM file exists at a guessed path instead of reading the convention `/taskfoundry:mom-init` recorded.

See `references/template-plan-structure.md` for the full template, including the progress tracker format and where the `Source` field goes.
