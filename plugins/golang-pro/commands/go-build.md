---
description: Fix Go build errors, go vet warnings, and linter issues incrementally. Invokes the go-build-resolver agent for minimal, surgical fixes.
---

# Go Build and Fix

Invokes the **go-build-resolver** agent to incrementally fix Go build errors with minimal changes.

## What This Command Does

1. Run Diagnostics: `go build`, `go vet`, `staticcheck`, `golangci-lint`, `go mod verify`
2. Parse Errors: Group by file and sort by severity
3. Fix Incrementally: One error at a time
4. Verify Each Fix: Re-run build after each change
5. Report Summary: Show what was fixed and what remains

## When to Use

- `go build ./...` fails with errors
- `go vet ./...` reports issues
- `golangci-lint run` shows warnings
- Module dependencies are broken
- After pulling changes that break the build

## Fix Strategy

1. Build errors first — code must compile
2. Vet warnings second — fix suspicious constructs
3. Lint warnings third — style and best practices
4. One fix at a time — verify each change
5. Minimal changes — don't refactor, just fix

## Common Errors Fixed

| Error | Typical Fix |
|-------|-------------|
| `undefined: X` | Add import or fix typo |
| `cannot use X as Y` | Type conversion or fix assignment |
| `missing return` | Add return statement |
| `X does not implement Y` | Add missing method |
| `import cycle` | Restructure packages |
| `declared but not used` | Remove or use variable |
| `cannot find package` | `go get` or `go mod tidy` |

## Stop Conditions

The agent stops if:
- Same error persists after 3 attempts
- Fix introduces more errors
- Requires architectural changes
- Missing external dependencies

## Related

- `/go-test` — Run tests after build succeeds
- `/go-review` — Review code quality
- Agent: `agents/go-build-resolver.md`
- Skill: `skills/golang-patterns/`
