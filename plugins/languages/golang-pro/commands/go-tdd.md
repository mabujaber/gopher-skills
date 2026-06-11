---
description: Enforce test-driven development for Go. Scaffold tests first, implement second. Applies the golang-testing skill for idiomatic patterns and coverage.
---

# Go TDD

Enforces a test-driven development workflow for Go code. Apply the **golang-testing** skill — its `references/details.md` has complete worked examples for table-driven tests, parallel subtests, and coverage tooling. Do not duplicate those examples here.

## When to Use

- Implementing new Go functions
- Adding test coverage to existing code
- Fixing bugs (write failing test first)
- Building critical business logic

## TDD Cycle

```
RED     → Write failing table-driven test
GREEN   → Implement minimal code to pass
REFACTOR → Improve code, tests stay green
REPEAT  → Next test case
```

## 6-Step Workflow

1. Define Types/Interfaces: Scaffold function signatures first
2. Write Table-Driven Tests: Create comprehensive test cases (RED)
3. Run Tests: Verify tests fail for the right reason
4. Implement Code: Write minimal code to pass (GREEN)
5. Refactor: Improve while keeping tests green
6. Check Coverage: Ensure 80%+ coverage

## Related

- `/go-build` — Fix build errors
- `/go-review` — Review code after implementation
- Skill: `skills/golang-testing/`
- Skill: `skills/golang-patterns/`
