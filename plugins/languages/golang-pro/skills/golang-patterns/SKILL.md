---
name: golang-patterns
description: >
  Use when writing, reviewing, or refactoring Go code and idiomatic patterns are needed — error wrapping with %w, errors.Is/errors.As, interface design, package layout, functional options, sync.Pool, strings.Builder, golangci-lint setup, anti-patterns (panic for control flow, naked returns, mixed receivers).
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(go:*), Bash(gofmt:*), Bash(golangci-lint:*)
version: 1.0.0
license: MIT
tags:
  - go-ecosystem
  - golang
  - idioms
  - patterns
compatibility: Designed for Claude Code.
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

Concurrency patterns (worker pools, errgroup, graceful shutdown, leak avoidance) are covered in depth by the go-concurrency-patterns skill; this skill's references/details.md also carries the code examples.

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

## Detailed Reference

Full code examples for every pattern above (error wrapping, worker pools, functional options, interface design, memory optimization, linter config) live in `references/details.md`. Read that file when implementing a pattern rather than just naming it.
