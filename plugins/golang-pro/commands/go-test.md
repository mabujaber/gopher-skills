---
description: Enforce TDD workflow for Go. Write table-driven tests first, then implement. Verify 80%+ coverage with go test -cover.
---

# Go TDD Command

Enforces test-driven development methodology for Go code using idiomatic Go testing patterns.

## What This Command Does

1. Define Types/Interfaces: Scaffold function signatures first
2. Write Table-Driven Tests: Create comprehensive test cases (RED)
3. Run Tests: Verify tests fail for the right reason
4. Implement Code: Write minimal code to pass (GREEN)
5. Refactor: Improve while keeping tests green
6. Check Coverage: Ensure 80%+ coverage

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

## Test Patterns

Table-driven tests:
```go
tests := []struct {
    name    string
    input   string
    want    string
    wantErr bool
}{
    {"valid email", "user@example.com", "user@example.com", false},
    {"missing @", "invalid", "", true},
    {"empty", "", "", true},
}
for _, tt := range tests {
    t.Run(tt.name, func(t *testing.T) {
        got, err := Validate(tt.input)
        if (err != nil) != tt.wantErr {
            t.Errorf("wantErr=%v, got err=%v", tt.wantErr, err)
        }
        if got != tt.want {
            t.Errorf("got %q, want %q", got, tt.want)
        }
    })
}
```

Parallel subtests (always capture loop var):
```go
for _, tt := range tests {
    tt := tt
    t.Run(tt.name, func(t *testing.T) {
        t.Parallel()
        // ...
    })
}
```

## Coverage Commands

```bash
go test -cover ./...
go test -coverprofile=coverage.out ./...
go tool cover -html=coverage.out
go tool cover -func=coverage.out
go test -race -cover ./...
```

## Coverage Targets

| Code Type | Target |
|-----------|--------|
| Critical business logic | 100% |
| Public APIs | 90%+ |
| General code | 80%+ |
| Generated code | Exclude |

## Related

- `/go-build` — Fix build errors
- `/go-review` — Review code after implementation
- Skill: `skills/golang-testing/`
- Skill: `skills/golang-patterns/`
