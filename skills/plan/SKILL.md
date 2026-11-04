---
name: plan
description: Use when turning meeting notes, issues, or mastersheet items into a dev, design, or test plan, before starting execution.
---

# Daily Plan

## Overview
Turn today's tasks into a plan no bigger than the task, shaped by the role doing the work. Every plan records where the task came from, so `/taskfoundry:develop` never has to guess.

## When to Use
- After `/taskfoundry:summary`, once new or leftover tasks for today are identified.
- For multi-day work, split into batches with one plan per batch instead of growing a plan past its limit.

## Sizes

| `size=` | When | Contents | Max lines |
|---------|------|----------|-----------|
| `none` | About an hour or less, and already clear | `Source` and done criteria, in chat | 0 (no file) |
| `light` (default) | One PR or deliverable | Header, Tracker, Acceptance, Out of scope, Decisions | 40 |
| `full` | Cross-repo, risky, or requested | Light plus Analysis and notes per area | 120 |

## Roles

| `role=` | Read first | Template |
|---------|------------|----------|
| `dev` | The code the task touches | `references/template-plan-dev.md` |
| `design` | Knowledge docs, WBS, earlier prototypes | `references/template-plan-design.md` |
| `test` | Requirements, issues, API contracts | `references/template-plan-test.md` |

## Process
1. **Identify the tasks**: from MOM entries, issues, or mastersheet items. Read their locations from the root `CLAUDE.md` (the project's top-level one; in a single-repo project, the repo's own). If nothing is recorded, ask the user.
2. **Pick role and size**: from the `role=` and `size=` arguments, otherwise infer them from the task. If the role can't be inferred, ask once. Size defaults to `light`.
3. **Read the sources** listed for that role — don't plan blind.
4. **Clarify ambiguous tasks**: one question at a time, don't assume. Record what was settled in `Decisions`, 3-5 lines at most.
5. **Write the plan**:
   - `none`: reply in chat with `Source` and the done criteria, and write no file. `/taskfoundry:develop` carries them into the PR body.
   - `light` or `full`: read only the template for this role, not the other two. Delete the full-only part for `light`.
   - The header needs `Source`, `Role`, `Size`, and `Status`. `Source` is a mastersheet link, ticket ID, issue path, or `direct request`.
   - In `Depends`, list only real ordering between tasks; `/taskfoundry:orchestrate` runs on it.
   - Add any project-specific header fields the root `CLAUDE.md` requires. Never copy code or signatures into the plan.
6. **Save it** where the root `CLAUDE.md` says. If nothing is recorded, propose `docs/taskfoundry/plans/YYYY-MM-DD-<topic>.md`, wait for the user's yes or override, and record the answer there. Never overwrite an existing plan; give the new one a different topic slug.
7. **Check the limit**: `wc -l` the file. Over the limit means trim it or split it into batches, not raise the limit.

## Common Mistakes
- Silently applying the default path without confirmation.
- Skipping `Source` — it's the only reason `/taskfoundry:develop` can fill the PR's `Related` section truthfully.
- Writing a `dev` plan's tracker per file, or forcing file lists onto `design` and `test` plans.
- Loading all three templates when only one role applies.
- Assuming a MOM file exists at a guessed path instead of reading the location in the root `CLAUDE.md`.
