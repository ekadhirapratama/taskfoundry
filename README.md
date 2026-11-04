# taskfoundry

Daily solo-dev workflow as 5 Claude Code slash commands: log meetings, review what happened, plan what's next, execute it — consistently, across every repo you touch.

## Install

```
/plugin marketplace add ekadhirapratama/taskfoundry
/plugin install taskfoundry@taskfoundry
```

## Commands

- **`/taskfoundry:summary`** — start-of-day reconciliation: compares actual git/PR activity against your mastersheet (or today's sync file, if you have one) and prior meeting notes, classifies every task as done/on-going/todo, and replies with a status summary for the daily meeting in 25 lines at most.
- **`/taskfoundry:mom-init`** — right after a meeting ends, logs decisions and new tasks into a consistently-formatted, dated MOM entry that `summary` and `plan` both know how to find. `issue` mode logs one direct request or decision the same way, in 25 lines at most.
- **`/taskfoundry:plan`** — turns today's new tasks into a plan sized to the task (`size=none`, `light` at 40 lines or fewer, or `full` at 120 or fewer) and shaped by the role (`role=dev`, `design`, or `test`), with every plan recording where the task came from (`Source` field) so the PR description never has to guess.
- **`/taskfoundry:develop`** — works the plan to completion: splits tasks by dependency, runs the done-check for the plan's role, updates the progress tracker, commits, and opens the PR — asking you directly for anything (like a preview screenshot) it can't know on its own.
- **`/taskfoundry:orchestrate`** — runs a plan's open tasks through isolated workers, at most 3 at a time: subagents for short tasks in one repo, or [Orca](https://www.onorca.dev) worktrees for cross-repo or long work, falling back to subagents when Orca isn't available. Updates the tracker and stops before any push or PR.

## Why

Solo devs and small teams juggling multiple client repos tend to reinvent this ritual by hand every morning. taskfoundry makes it a fixed, repeatable set of commands instead.

## License

MIT — see [LICENSE](LICENSE).
