---
name: develop
description: Use when a plan from today is ready to carry out, through to its role's done-check and the pull request.
---

# Daily Execution

## Overview
Work today's plan to completion: split tasks by dependency, verify each against its acceptance criteria and the plan's role, keep the progress tracker current, then open the PR without guessing at sections the plan doesn't answer.

## When to Use
- After `/taskfoundry:plan` has produced a plan, or settled on `size=none` in chat.
- Not for planning — if there's no plan yet, use `/taskfoundry:plan` first.

## Done-check per role

| `Role` | Done when |
|--------|-----------|
| `dev` (also when the plan has no `Role`) | Acceptance met; lint and build pass |
| `design` | Acceptance met; link or PNG ready; approval recorded in the plan |
| `test` | Report filled in; env recorded |

## Process
1. **Classify tasks** from the tracker: independent tasks can run in parallel, dependent tasks must run chained. Work them one at a time here. To run independent tasks in parallel, or work spanning several repos, use `/taskfoundry:orchestrate`, then come back here for the PR.
2. **Run the role's done-check on each finished task.** If it fails, stop — fix or report before committing. Never commit past a failing check.
3. **Update the plan's progress tracker** (the same file `/taskfoundry:plan` created) to reflect what's actually done. Skip this for `size=none`, which has no file.
4. **Commit** using this detection order on the target repo:
   1. Explicit config — `CONTRIBUTING.md`, `.commitlintrc*`, a `commitlint` block in `package.json`, or the project's `CLAUDE.md`. If found, follow it.
   2. Recent history — `git log --oneline -20`. If commits show a consistent pattern, follow it.
   3. Fallback — Conventional Commits, per `references/commit-convention.md`.
5. **Once every task in the tracker is done**: if the work changed a repo, push and `gh pr create` using *that repo's own* `pull_request_template.md`. For `size=none`, put `Source` and the done criteria in the PR body. Two sections need an explicit rule instead of a guess:
   - **Related**: copy from the plan's `Source` field. If it isn't filled in, write `-` and ask the user for the source — never invent one.
   - **Preview**: this section holds a screenshot or URL that only the user has. Always leave it blank in the draft and explicitly ask the user for it before submitting. Never fill it with an assumed image or description.

## Common Mistakes
- Filling `Related` or `Preview` from assumption when the plan doesn't have the answer — ask the user instead.
- Committing with a failing check "to fix in the next commit."
- Holding `design` or `test` work to a lint/build gate it doesn't have.
- Skipping convention detection and always forcing Conventional Commits, even when the repo has its own established pattern.
