---
description: Comprehensive Go code review for idiomatic patterns, concurrency safety, error handling, and security. Invokes the go-reviewer agent.
---

# Go Code Review

Invokes the **go-reviewer** agent for comprehensive Go-specific code review.

## What This Command Does

1. Identify Go Changes: Find modified `.go` files via `git diff`
2. Run Static Analysis: `go vet`, `staticcheck`, `golangci-lint`
3. Security Scan: Check for SQL injection, command injection, race conditions
4. Concurrency Review: Analyze goroutine safety, channel usage, mutex patterns
5. Idiomatic Go Check: Verify code follows Go conventions
6. Generate Report: Categorize issues by severity

## When to Use

- After writing or modifying Go code
- Before committing Go changes
- Reviewing pull requests with Go code
- Onboarding to a new Go codebase

## Review Categories

### CRITICAL (Must Fix)
- SQL/Command injection vulnerabilities
- Race conditions without synchronization
- Goroutine leaks
- Hardcoded credentials
- Unsafe pointer usage
- Ignored errors in critical paths

### HIGH (Should Fix)
- Missing error wrapping with context
- Panic instead of error returns
- Context not propagated
- Unbuffered channels causing deadlocks
- Missing mutex protection

### MEDIUM (Consider)
- Non-idiomatic code patterns
- Missing godoc comments on exports
- Inefficient string concatenation
- Slice not preallocated
- Table-driven tests not used

## Approval Criteria

| Status | Condition |
|--------|-----------|
| PASS | No CRITICAL or HIGH issues |
| WARNING | Only MEDIUM issues — merge with caution |
| FAIL | CRITICAL or HIGH issues found |

## Related

- Use `/go-test` first to ensure tests pass
- Use `/go-build` if build errors occur
- Use `/go-review` before committing
- Agent: `agents/go-reviewer.md`
- Skills: `skills/golang-patterns/`, `skills/golang-testing/`
