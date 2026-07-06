# nats

A Claude Code plugin for developing with [NATS](https://nats.io) — the connective technology for
distributed systems. Covers core NATS, JetStream, key-value/object stores, security, clustering,
leaf nodes, server deployment/embedding, the Services (micro) framework, and the NATS CLI.

## What's included

A single `nats` skill (Go-centric, applicable across the 40+ NATS client libraries):

- **Core NATS** — subjects and wildcards, pub/sub, request/reply, queue groups
- **JetStream** — streams, publishing (async + exactly-once), pull/push consumers, delivery & ack policies
- **Key/Value & Object stores** — built on JetStream
- **Connections** — reconnect logic, credentials, URL schemes
- **Security** — token, user/pass, NKey, JWT/operator model, TLS, auth callouts, accounts, permissions
- **Server** — configuration, clustering (R3/R5), leaf nodes, embedding in Go
- **Services API** — discoverable micro services over Core NATS
- **Subject mapping/transforms** — canary, partitioning
- **Monitoring** — HTTP endpoints, `nats-top`
- **NATS CLI** — contexts, streams, consumers, KV, benchmarks
- **Common mistakes** — `nc.Publish` vs `js.Publish`, `Drain` vs `Close`, etc.

## Optional companion

For live operations, pair this skill with [`jesseobrien/nats-mcp`](https://github.com/jesseobrien/nats-mcp)
(MIT) — a Go MCP server exposing 42 NATS tools (core, JetStream, KV, object store). This plugin
ships guidance only, not the MCP runtime.

## Attribution

Ported from [`kaustavdm/nats-skill`](https://github.com/kaustavdm/nats-skill) (MIT). See
[NOTICE](NOTICE) for provenance and [LICENSE](LICENSE) for terms.
