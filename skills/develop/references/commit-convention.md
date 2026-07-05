# Commit Convention Fallback: Conventional Commits

Use when the target repo has no explicit config (`CONTRIBUTING.md`,
`.commitlintrc*`, `commitlint` block in `package.json`) and no consistent
pattern in `git log --oneline -20`.

Source of truth: https://www.conventionalcommits.org/en/v1.0.0/

## Format

```
type(scope): description
```

Scope is optional.

## Common types

- `feat` — new feature
- `fix` — bug fix
- `docs` — documentation only
- `style` — formatting, no code change
- `refactor` — code change that's neither a fix nor a feature
- `test` — adding/correcting tests
- `chore` — maintenance (deps, tooling, config)
- `perf` — performance improvement
- `build` — build system or external dependencies
- `ci` — CI configuration

## Breaking changes

Mark with `!` after type/scope, or add a `BREAKING CHANGE:` footer:

```
feat(api)!: remove deprecated /v1 endpoint

BREAKING CHANGE: /v1 endpoint removed, use /v2 instead.
```
