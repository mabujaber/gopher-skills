---
name: temporal-developer
description: >
  Develop, debug, and manage Temporal applications across Python, TypeScript, Go, and Java.
  Use when building workflows, activities, or workers with a Temporal SDK, debugging issues
  like non-determinism errors, stuck workflows, or activity retries, using Temporal CLI,
  Temporal Server, or Temporal Cloud, or working with durable execution concepts like
  signals, queries, heartbeats, versioning, continue-as-new, child workflows, or saga patterns.
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(temporal:*), Bash(brew install temporal)
version: 1.0.0
license: Apache-2.0
tags:
  - temporal
  - workflows
  - durable-execution
  - distributed-systems
  - python
  - typescript
  - go
  - java
compatibility: Designed for Claude Code. Ported from the Temporal temporal-developer Codex skill.
---

# Skill: temporal-developer

## Overview

Temporal is a durable execution platform that makes workflows survive failures automatically.
This skill provides guidance for building Temporal applications in Python, TypeScript, Go, and Java.

## Core Architecture

The **Temporal Cluster** is the central orchestration backend. It maintains three key subsystems:
the **Event History** (a durable log of all workflow state), **Task Queues** (which route work to
the right workers), and a **Visibility** store (for searching and listing workflows). There are
three ways to run a Cluster:

- **Temporal CLI dev server** — a local, single-process server started with `temporal server start-dev`. Suitable for development and testing only, not production.
- **Self-hosted** — you deploy and manage the Temporal server and its dependencies in your own infrastructure.
- **Temporal Cloud** — a fully managed production service operated by Temporal.

**Workers** are long-running processes that poll Task Queues for work and execute your code. Each Worker hosts:

- **Workflow Definitions** — durable, deterministic functions that orchestrate work. Must not have side effects.
- **Activity Implementations** — non-deterministic operations (API calls, file I/O, etc.) that can fail and be retried.

## History Replay: Why Determinism Matters

Temporal achieves durability through **history replay**:

1. **Initial Execution** - Worker runs workflow, generates Commands, stored as Events in history
2. **Recovery** - On restart/failure, Worker re-executes workflow from beginning
3. **Matching** - SDK compares generated Commands against stored Events
4. **Restoration** - Uses stored Activity results instead of re-executing

**If Commands don't match Events = Non-determinism Error = Workflow blocked**

| Workflow Code | Command | Event |
|--------------|---------|-------|
| Execute activity | `ScheduleActivityTask` | `ActivityTaskScheduled` |
| Sleep/timer | `StartTimer` | `TimerStarted` |
| Child workflow | `StartChildWorkflowExecution` | `ChildWorkflowExecutionStarted` |

See `references/core/determinism.md` for detailed explanation.

## Getting Started

### Ensure Temporal CLI is installed

Check if `temporal` CLI is installed. If not:

**macOS:** `brew install temporal`

**Linux:** Download from https://temporal.download/cli/archive/latest?platform=linux&arch=amd64 (or arm64), extract, and add to PATH.

**Windows:** Download from https://temporal.download/cli/archive/latest?platform=windows&arch=amd64 (or arm64), extract, and add to PATH.

### Read All Relevant References

1. First, read the getting started guide for the language you are working in:
    - Python → read `references/python/python.md`
    - TypeScript → read `references/typescript/typescript.md`
    - Java → read `references/java/java.md`
    - Go → read `references/go/go.md`
2. Second, read appropriate `core` and language-specific references for the task at hand.

## Primary References
- **`references/core/determinism.md`** - Why determinism matters, replay mechanics, basic concepts of activities
    + Language-specific info at `references/{your_language}/determinism.md`
- **`references/core/patterns.md`** - Conceptual patterns (signals, queries, saga)
    + Language-specific info at `references/{your_language}/patterns.md`
- **`references/core/gotchas.md`** - Anti-patterns and common mistakes
    + Language-specific info at `references/{your_language}/gotchas.md`
- **`references/core/versioning.md`** - Versioning strategies and concepts
    + Language-specific info at `references/{your_language}/versioning.md`
- **`references/core/troubleshooting.md`** - Decision trees, recovery procedures
- **`references/core/error-reference.md`** - Common error types, workflow status reference
- **`references/core/interactive-workflows.md`** - Testing signals, updates, queries
- **`references/core/dev-management.md`** - Dev cycle & management of server and workers
- **`references/core/ai-patterns.md`** - AI/LLM pattern concepts
    + Language-specific info at `references/{your_language}/ai-patterns.md`, if available. Currently Python only.

## Additional Topics
- **`references/{your_language}/observability.md`** - Language-specific observability guidance
- **`references/{your_language}/advanced-features.md`** - Advanced Temporal features

## Feedback

If you find this skill's explanations are unclear, misleading, or missing important information,
please file an issue at https://github.com/temporalio/skill-temporal-developer/issues/new.
