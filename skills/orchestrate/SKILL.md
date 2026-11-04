---
name: orchestrate
description: Use when a plan's tracker has two or more open tasks that could run in parallel, or tasks spanning several repos or a long session.
---

# Orchestrate

## Overview
Run a plan's open tasks through isolated workers, at most 3 at a time, and keep the tracker accurate. Stop before anything leaves this machine: no push, PR, or merge. Project rules come from the root `CLAUDE.md` (the project's top-level one; in a single-repo project, the repo's own), and every worker is a Claude agent.

## When to Use
- A plan from `/taskfoundry:plan` has a tracker with work that can run side by side.
- Not for one task or a strictly sequential plan — use `/taskfoundry:develop`.
- Not without a tracker — use `/taskfoundry:plan` first.

## Process
1. **Read the rules and the plan**: the root `CLAUDE.md`, any rule files it or `.claude/rules/` apply to the target repos, then the plan's `Role`, `Acceptance`, and tracker. A plan with no `Role` is `dev`.
2. **Split the open tasks** (`[ ]`):
   - *Dependent*: `Depends` names an unfinished task, or it edits the same files or generated output as another task. When unsure, treat it as dependent.
   - *Independent*: everything else. Start up to 3 as the first wave; the rest wait for a free slot.
   - A dependent task starts once its dependency is verified, based on that dependency's branch.
3. **Pick the route** and say which one in chat:

   | Work | Route |
   |------|-------|
   | Short task in one repo | Subagent |
   | Spans repos, or a long session | Orca |
   | Orca unavailable or its experimental orchestration is off | Subagent |

4. **Write a self-contained spec per worker** using the format below. A worker can't see this conversation.
5. **Start and monitor** (see the route's section). Mark a started task `[~]` in the tracker.
6. **Verify each settled worker yourself**: in its worktree, check `git log <base>..HEAD` and `git status`, and run the role's done-check from `/taskfoundry:develop`. Don't take the worker's report as proof.
7. **Update the tracker** right after each verification. Only the coordinator edits it. `[x]` with branch and commit in `Notes`, or `[!]` with the reason. Tasks depending on a `[!]` task stay `[ ]`.
8. **Stop**: report per task the outcome, evidence, branch, and worktree path, plus anything blocked. Leave worktrees and branches in place. Pushing and opening the PR wait for the user's approval, then go through `/taskfoundry:develop`.

## Worker spec

```text
Read first: <absolute path of the root CLAUDE.md>; <absolute paths of the rule files that apply to this target>
Target: <repo, worktree or base branch, files or area in scope>
Change: <the concrete result>
Constraints: <rules that apply, in the project's words; what not to touch>
Ownership: edit only the target. Do not edit the plan or tracker.
Acceptance: <the task's criteria and the role's done-check, each with its evidence>
Finish: commit on this worktree's branch following the project's convention. Do not push, open a PR or MR, or merge. Report outcome, branch, commit, and evidence.
```

Fill `Read first` with paths resolved from the root `CLAUDE.md`. A worker in a worktree may not load the root `CLAUDE.md` or path-scoped rules on its own, so never rely on that.

## Route: subagent
Start one subagent per task, using the agent the root `CLAUDE.md` routes this role to, or the general-purpose agent if it names none. Give each its own worktree: `isolation: "worktree"` when the session is inside the target repo and the task starts from its default branch. For another repo or a dependency's branch, first run `git -C <repo> worktree add <repo>/.claude/worktrees/<slug> -b <branch> <base>` (a sibling of the repo may be outside what the session can write; a repo outside it needs the user's access first), name that path in `Target`, and tell the worker to run `git -C <path>` with the literal path, not `cd` or a shell variable, which can trigger permission prompts it can't answer.

## Route: Orca
Check availability, then create the Run. If `orca` is missing, the status isn't ready, or `run-create` fails, take the subagent route and say so.

```text
orca status --json                # result.runtime.state must be "ready"
orca orchestration run-create --objective "<plan title>" --json
orca orchestration worker-start --spec "<spec>" --task-title "<ID> <task>" --worktree new-child --name <slug> [--repo <selector>] [--base-branch <ref>] --agent claude --json
orca orchestration check --wait --types "worker_done,escalation,question" --timeout-ms 900000 --json
orca orchestration reply --id <message_id> --body "<answer>" --json
orca orchestration worker-release --dispatch <dispatch_id> --json
orca orchestration check --ack <delivery_id> --wait --types "worker_done,escalation,question" --timeout-ms 900000 --json
```

- Start the whole wave before the first wait. Pass `--model` only if the user named one.
- Process each delivery: answer questions, verify and update the tracker on `worker_done`, then `worker-release`, then `--ack`. Release closes the terminal and keeps the worktree.
- A timeout or empty wait is a checkpoint, not a failure. After three in a row, read `orca orchestration worker-list --run <run_id> --json`. Never stop or retry a worker on silence alone.
- If `worker-start` exits non-zero, don't relaunch or fall back: the worker may partly exist. Read `failedStage`, then run `orca skills get orchestration --full`.
- On any other Orca error or unfamiliar state, run `orca skills get orchestration --full` and follow its recovery section.
- Finished when `orca orchestration worker-list --run <run_id> --terminal-state reclaimable --json` returns none.

## Common Mistakes
- Pushing or opening a PR because every task is done — that's the user's call.
- Marking `[x]` from a worker's report without checking its worktree.
- Letting workers edit the tracker, which causes conflicts across worktrees.
- Writing "follow CLAUDE.md" in a spec without its absolute path.
- Copying project names or paths into this skill — they belong in the root `CLAUDE.md`.
