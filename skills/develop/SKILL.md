---
name: develop
description: Use when implementing tasks from today's plan doc — splitting independent vs dependent work, checking acceptance criteria and lint, and opening the pull request.
---

# Daily Execution

## Overview
Work today's plan doc to completion: split tasks by dependency, verify each against acceptance criteria and lint/build, keep the plan's progress tracker current, then open the PR without guessing at sections the plan doc doesn't answer.

## When to Use
- After `/taskfoundry:plan` has produced a plan doc with a progress tracker.
- Not for planning — if there's no plan doc yet, use `/taskfoundry:plan` first.

## Process
1. **Classify tasks** from the plan doc's progress tracker: independent tasks can run in parallel, dependent tasks must run chained.
   If the `superpowers` plugin is installed, use `superpowers:dispatching-parallel-agents` to dispatch one subagent per independent task. If it isn't installed, work independent tasks one at a time instead of inventing a parallel-dispatch mechanism.
2. **Verify each finished task** against its acceptance criteria in the plan doc.
3. **Check lint/build**: if either fails, stop — fix or report before committing. Never commit past a failing check.
4. **Update the plan doc's progress tracker** (the same file `/taskfoundry:plan` created) to reflect what's actually done.
5. **Commit** using this detection order on the target repo:
   1. Explicit config — `CONTRIBUTING.md`, `.commitlintrc*`, or a `commitlint` block in `package.json`. If found, follow it.
   2. Recent history — `git log --oneline -20`. If commits show a consistent pattern, follow it.
   3. Fallback — Conventional Commits, per `references/commit-convention.md`.
6. **Once every task in the tracker is done**: push and `gh pr create` using *that repo's own* `pull_request_template.md`. Two sections need an explicit rule instead of a guess:
   - **Related**: copy from the plan doc's `Source` field. If the plan doc has no `Source` field filled in, write `-` and ask the user for the source — never invent one.
   - **Preview**: this section holds a screenshot or URL that only the user has. Always leave it blank in the draft and explicitly ask the user for it before submitting. Never fill it with an assumed image or description.

## Common Mistakes
- Filling `Related` or `Preview` from assumption when the plan doc doesn't have the answer — ask the user instead.
- Committing with failing lint/build "to fix in the next commit."
- Assuming `dispatching-parallel-agents` is available without checking whether the `superpowers` plugin is installed.
- Skipping convention detection and always forcing Conventional Commits, even when the repo has its own established pattern.
