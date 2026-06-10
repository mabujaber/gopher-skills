---
name: golang-patterns
description: Idiomatic Go patterns, best practices, and conventions for building robust, efficient, and maintainable Go applications.
origin: ECC
---

# Go Development Patterns

Idiomatic Go patterns and best practices for building robust, efficient, and maintainable applications.

## When to Activate

- Writing new Go code
- Reviewing Go code
- Refactoring existing Go code
- Designing Go packages/modules

## Core Principles

### 1. Simplicity and Clarity
Go favors simplicity over cleverness. Code should be obvious and easy to read.

### 2. Make the Zero Value Useful
Design types so their zero value is immediately usable without initialization.

### 3. Accept Interfaces, Return Structs
Functions should accept interface parameters and return concrete types.

## Error Handling Patterns

- **Error Wrapping with Context**: `fmt.Errorf("context: %w", err)`
- **Custom Error Types**: domain-specific errors + sentinel errors
- **Error Checking**: use `errors.Is` and `errors.As`, not `==`
- **Never Ignore Errors**: never discard errors with `_`

## Concurrency Patterns

- Worker Pool (WaitGroup-based)
- Context for Cancellation and Timeouts
- Graceful Shutdown (OS signal listening)
- `errgroup` for Coordinated Goroutines
- Avoiding Goroutine Leaks (buffered channels + select on ctx.Done)

## Interface Design

- Small, Focused Interfaces (single-method preferred)
- Define Interfaces Where They're Used (consumer package, not provider)
- Optional Behavior with Type Assertions

## Package Organization

- Standard Project Layout (`cmd/`, `internal/`, `pkg/`, `api/`, `testdata/`)
- Package Naming: short, lowercase, no underscores
- Avoid Package-Level State (prefer dependency injection)

## Struct Design

- Functional Options Pattern
- Embedding for Composition

## Memory and Performance

- Preallocate Slices When Size is Known: `make([]T, 0, cap)`
- Use `sync.Pool` for Frequent Allocations
- Avoid String Concatenation in Loops: use `strings.Builder` or `strings.Join`

## Go Tooling Integration

Essential commands: `go build`, `go test`, `go vet`, `staticcheck`, `golangci-lint`, `gofmt`, `goimports`

## Quick Reference: Go Idioms

| Idiom | Rule |
|-------|------|
| Accept interfaces, return structs | Decouple callers from implementations |
| Errors are values | Handle them explicitly, not with exceptions |
| Don't communicate by sharing memory | Share memory by communicating |
| Make the zero value useful | No forced initialization |
| A little copying is better than a little dependency | Avoid premature abstraction |
| Clear is better than clever | Prioritize readability |
| `gofmt` is non-negotiable | All code must be formatted |
| Return early | Avoid deep nesting with guard clauses |

## Anti-Patterns to Avoid

- Naked returns in long functions
- Using `panic` for control flow
- Passing context in struct (use as first parameter instead)
- Mixing value and pointer receivers on the same type
