---
description: Comprehensive Go code review for idiomatic patterns, concurrency safety, error handling, and security. Invokes the go-reviewer agent.
---

# Go Code Review

Launch the **go-reviewer** agent via the Task tool. The agent owns severity categories, approval criteria, static analysis steps, and concurrency review patterns — refer to `agents/go-reviewer.md` for full detail.

## When to Use

- After writing or modifying Go code
- Before committing Go changes
- Reviewing pull requests with Go code
- Onboarding to a new Go codebase

## Related

- Use `/go-tdd` first to ensure tests pass
- Use `/go-build` if build errors occur
- Agent: `agents/go-reviewer.md`
- Skills: `skills/golang-patterns/`, `skills/golang-testing/`
