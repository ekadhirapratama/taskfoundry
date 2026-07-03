# taskfoundry

Daily solo-dev workflow as 4 Claude Code slash commands: log meetings, review what happened, plan what's next, execute it — consistently, across every repo you touch.

## Install

```
/plugin marketplace add ekadhirapratama/taskfoundry
/plugin install taskfoundry@taskfoundry
```

## Commands

- **`/taskfoundry:summary`** — start-of-day reconciliation: compares actual git/PR activity against your mastersheet and prior meeting notes, classifies every task as done/on-going/todo, and preps a status summary for the daily meeting.
- **`/taskfoundry:mom-init`** — right after a meeting ends, logs decisions and new tasks into a consistently-formatted, dated MOM entry that `summary` and `plan` both know how to find.
- **`/taskfoundry:plan`** — turns today's new tasks (from MOM entries or the mastersheet) into a written plan doc using a consistent template, with every plan recording where the task came from (`Source` field) so the PR description never has to guess.
- **`/taskfoundry:develop`** — works the plan doc to completion: splits independent tasks to run in parallel, checks acceptance criteria and lint/build, updates the progress tracker, commits, and opens the PR — asking you directly for anything (like a preview screenshot) it can't know on its own.

## Why

Solo devs and small teams juggling multiple client repos tend to reinvent this ritual by hand every morning. taskfoundry makes it a fixed, repeatable set of commands instead.

## License

MIT — see [LICENSE](LICENSE).
