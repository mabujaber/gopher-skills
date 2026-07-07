# k6

A Claude Code plugin for writing, debugging, and running [k6](https://k6.io) load and performance
tests in JavaScript/TypeScript — from a single script up to an end-to-end website performance-testing
workflow.

## What's included

A single skill covering:

- **Scripting** — HTTP, WebSocket, gRPC, browser tests, custom metrics, `SharedArray`, lifecycle (`setup`/`teardown`).
- **Test design** — checks, thresholds (incl. `abortOnFail` and tag-based), scenarios, executors, and the six test types (smoke / load / stress / spike / soak / breakpoint).
- **Execution** — CLI flags, env vars, output formats, and Grafana Cloud k6.
- **End-to-end website workflow** — record real flows with Playwright, build functional protocol + browser tests, then hybrid load tests with SLO-backed thresholds and a load-generator monitor. Ships reusable scaffolds under `assets/`.

## When it activates

Triggers on k6, load testing, stress/spike/soak testing, thresholds, virtual users (VUs), performance
testing, or "perf test my site".

## Attribution

Ported from the official [`grafana/skills`](https://github.com/grafana/skills) k6 skills
(Apache-2.0). See [NOTICE](NOTICE) for provenance (including what was intentionally not ported) and
[LICENSE](LICENSE) for terms.
