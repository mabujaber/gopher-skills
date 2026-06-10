---
name: golang-testing
description: Go testing patterns including table-driven tests, subtests, benchmarks, fuzzing, and test coverage. Follows TDD methodology with idiomatic Go practices.
origin: ECC
---

# Go Testing Patterns

Comprehensive Go testing patterns for writing reliable, maintainable tests following TDD methodology.

## When to Activate

- Writing new Go functions or methods
- Adding test coverage to existing code
- Creating benchmarks for performance-critical code
- Implementing fuzz tests for input validation
- Following TDD workflow in Go projects

## TDD Workflow for Go

RED-GREEN-REFACTOR cycle:

1. Define interface/signature with `panic("not implemented")`
2. Write failing test (RED)
3. Run test, verify FAIL
4. Implement minimal code (GREEN)
5. Run test, verify PASS
6. Refactor, verify tests still pass

## Table-Driven Tests

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name    string
        a, b    int
        want    int
        wantErr bool
    }{
        {"positive", 2, 3, 5, false},
        {"negative", -1, -2, -3, false},
        {"zero", 0, 0, 0, false},
    }
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := Add(tt.a, tt.b)
            if got != tt.want {
                t.Errorf("Add(%d, %d) = %d, want %d", tt.a, tt.b, got, tt.want)
            }
        })
    }
}
```

## Subtests and Parallel Tests

```go
func TestSomething(t *testing.T) {
    for _, tt := range tests {
        tt := tt // capture loop variable
        t.Run(tt.name, func(t *testing.T) {
            t.Parallel()
            // test body
        })
    }
}
```

## Test Helpers

```go
func assertEqual[T comparable](t *testing.T, got, want T) {
    t.Helper()
    if got != want {
        t.Errorf("got %v, want %v", got, want)
    }
}
```

Use `t.Cleanup()` for resource teardown, `t.TempDir()` for temporary directories.

## Mocking with Interfaces

Define an interface, create a mock struct with function fields, inject in tests:

```go
type Fetcher interface {
    Fetch(url string) ([]byte, error)
}

type MockFetcher struct {
    FetchFn func(url string) ([]byte, error)
}

func (m *MockFetcher) Fetch(url string) ([]byte, error) {
    return m.FetchFn(url)
}
```

## Benchmarks

```go
func BenchmarkProcess(b *testing.B) {
    b.ResetTimer()
    for i := 0; i < b.N; i++ {
        Process(data)
    }
}
```

Run with: `go test -bench=. -benchmem ./...`

## Fuzzing (Go 1.18+)

```go
func FuzzParse(f *testing.F) {
    f.Add("seed input")
    f.Fuzz(func(t *testing.T, input string) {
        result, err := Parse(input)
        if err == nil && result == "" {
            t.Error("non-empty input produced empty result")
        }
    })
}
```

Run with: `go test -fuzz=FuzzParse -fuzztime=30s ./...`

## Test Coverage

```bash
go test -cover ./...
go test -coverprofile=coverage.out ./...
go tool cover -html=coverage.out
go tool cover -func=coverage.out
```

Coverage targets:
- Critical logic: 100%
- Public APIs: 90%+
- General code: 80%+
- Generated code: exclude

## HTTP Handler Testing

```go
func TestHandler(t *testing.T) {
    req := httptest.NewRequest(http.MethodGet, "/", nil)
    w := httptest.NewRecorder()
    MyHandler(w, req)
    if w.Code != http.StatusOK {
        t.Errorf("status = %d, want %d", w.Code, http.StatusOK)
    }
}
```

## CI/CD Integration

```yaml
- name: Test
  run: go test -race -coverprofile=coverage.out ./...
- name: Coverage gate
  run: |
    COVERAGE=$(go tool cover -func=coverage.out | grep total | awk '{print $3}' | tr -d '%')
    [ $(echo "$COVERAGE >= 80" | bc) -eq 1 ] || exit 1
```

## Best Practices

**DO:**
- Follow TDD: write tests first
- Use table-driven tests for multiple cases
- Test behavior, not implementation details
- Use `t.Helper()` in helper functions
- Use `t.Parallel()` where safe
- Use `t.Cleanup()` for teardown
- Use meaningful test names

**DON'T:**
- Test private functions directly
- Use `time.Sleep` in tests
- Ignore flaky tests
- Mock everything
- Skip error paths in tests
