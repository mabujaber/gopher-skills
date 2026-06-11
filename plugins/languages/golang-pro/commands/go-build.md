---
description: Fix Go build errors, go vet warnings, and linter issues incrementally. Invokes the go-build-resolver agent for minimal, surgical fixes.
---

# Go Build and Fix

Launch the **go-build-resolver** agent via the Task tool to fix the build. The agent owns the fix-patterns, diagnostic steps, and error classification — refer to `agents/go-build-resolver.md` for full detail.

## When to Use

- `go build ./...` fails with errors
- `go vet ./...` reports issues
- `golangci-lint run` shows warnings
- Module dependencies are broken
- After pulling changes that break the build

## Stop Conditions

The agent stops if:
- Same error persists after 3 attempts
- Fix introduces more errors
- Requires architectural changes
- Missing external dependencies

## Related

- `/go-tdd` — Run tests after build succeeds
- `/go-review` — Review code quality
- Agent: `agents/go-build-resolver.md`
- Skill: `skills/golang-patterns/`
