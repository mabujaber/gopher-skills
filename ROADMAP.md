# Roadmap

Planned plugins for gopher-skills, organized by domain.

## Go Language Development

| Plugin | Description | Contents |
|--------|-------------|----------|
| `golang-pro` | Idiomatic Go development — patterns, TDD, concurrency, gRPC, browser automation, DBOS | 7 skills · 3 agents · 3 commands |

## Workflows

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `temporal-developer` ✅ | [Temporal](https://temporal.io) | Durable execution platform | Python, TypeScript, Go, Java |

## Infrastructure & Kubernetes

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `argo-workflows` | [Argo Workflows](https://argoproj.github.io/workflows/) | Kubernetes-native container workflow engine | Go, Python (Hera), REST |

## Identity, Auth & Authorization

### Ory (vendor namespace)

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `ory/kratos` | [Ory Kratos](https://www.ory.sh/kratos/) | User identity & self-service auth | Go, TypeScript, Python, REST |
| `ory/hydra` | [Ory Hydra](https://www.ory.sh/hydra/) | OAuth2 & OIDC server | Go, REST (any language) |
| `ory/keto` | [Ory Keto](https://www.ory.sh/keto/) | Permissions & access control (Zanzibar model) | Go, TypeScript, Python, REST |
| `ory/oathkeeper` | [Ory Oathkeeper](https://www.ory.sh/oathkeeper/) | Identity & access proxy | Config-driven, any language |

### Other Identity Tools

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `openfga` ✅ | [OpenFGA](https://openfga.dev) | Fine-grained authorization (CNCF) | Go, Node.js, .NET, Python, Java |
| `zitadel` | [Zitadel](https://zitadel.com) | Cloud-native identity platform | Go, REST/gRPC (any language) |

## Messaging & Event Streaming

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `nats` | [NATS](https://nats.io) | High-performance messaging & streaming | Go, Python, JS, Java, .NET, Rust, 40+ |

## Observability

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `signoz` ✅ | [SigNoz](https://signoz.io) | OpenTelemetry-native observability platform | OpenTelemetry SDKs (all languages) |

### Grafana Observability Stack (LGTM) — Tier 1

The Go-native "LGTM" stack (Loki, Grafana, Tempo, Metrics) plus continuous profiling and real-user
monitoring. All server components are built in Go (Faro's RUM web SDK is TypeScript, backed by Go
services). Marked Tier 1 — strong existing Claude-ecosystem skill coverage (official Grafana skills
and MCP servers) makes these the next ports after the current batch lands.

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `grafana` | [Grafana](https://grafana.com/oss/grafana/) | Visualization, dashboards, provisioning, alerting | Grafana HTTP API, SDKs, provisioning |
| `prometheus` | [Prometheus](https://prometheus.io) | Metrics collection & scraping (CNCF, Go) | PromQL, client libraries (Go, Java, Python, ...) |
| `loki` | [Loki](https://grafana.com/oss/loki/) | Log aggregation | LogQL, Promtail, clients |
| `tempo` | [Tempo](https://grafana.com/oss/tempo/) | Distributed tracing (OpenTelemetry-native) | OpenTelemetry, TraceQL |
| `pyroscope` | [Pyroscope](https://grafana.com/oss/pyroscope/) | Continuous profiling | Profiling agents (Go pprof, eBPF) |
| `grafana-faro` | [Grafana Faro](https://grafana.com/docs/grafana-cloud/monitor-applications/frontend-observability/) | Real User Monitoring (RUM) | Faro Web SDK (JS/TS) |

## API Gateways & Reverse Proxies

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `traefik` | [Traefik](https://traefik.io) | Cloud-native reverse proxy & load balancer | Config-driven (YAML/TOML), REST API |
| `caddy` | [Caddy](https://caddyserver.com) | Automatic HTTPS web server & reverse proxy | Config-driven (Caddyfile/JSON API) |

## Media & Image Processing

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `imgproxy` | [imgproxy](https://imgproxy.net) | Fast, secure image processing server | HTTP API (any language) |

## Load & Performance Testing

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `k6` ✅ | [k6](https://k6.io) | Developer-centric load testing tool | JavaScript test scripts, Go extensions |

## Feature Flags

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `flipt` | [Flipt](https://flipt.io) | Self-hosted feature flag platform | Go, Python, JS, Ruby, Java, .NET, REST |

## Storage & Object Stores

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `seaweedfs` | [SeaweedFS](https://github.com/seaweedfs/seaweedfs) | Distributed object/blob store & filer; billions of files with O(1) disk access | S3 API, REST/HTTP, FUSE, HDFS, WebDAV, gRPC (Go) |

## Status Legend

| Status | Meaning |
|--------|---------|
| ✅ Available | Plugin is complete and installable |
| 🚧 In Progress | Actively being built |
| 📋 Planned | On the roadmap, not started |

## Priority Order

### Tier 1 — Grafana observability stack (LGTM)

The full Go-native observability stack: dashboards, metrics, logs, traces, profiles, and RUM.
Strong existing Claude-ecosystem skill coverage (official Grafana skills + MCP servers), so these
are the next ports after the current Tier-1 batch (OpenFGA, k6, SigNoz, NATS, Flipt) lands.

1. **Grafana** — dashboards, visualization, provisioning, alerting
2. **Prometheus** — metrics collection & PromQL (CNCF)
3. **Loki** — log aggregation & LogQL
4. **Tempo** — distributed tracing & TraceQL
5. **Pyroscope** — continuous profiling
6. **Grafana Faro** — real user monitoring (RUM)

### Tier 2+ — remaining roadmap

7. **NATS** — broadest language coverage (40+ SDKs), huge community
8. **Ory/Kratos** — most-used Ory product, clear developer skill surface
9. **Traefik** — widely deployed, complex config surface benefits from a skill
10. **OpenFGA** — growing fast with AI authorization use cases
11. **k6** — every team needs load testing
12. **SigNoz** — OpenTelemetry-native, fills observability gap
13. **Flipt** — self-hosted feature flags, strong SDK coverage
14. **imgproxy** — niche but highly focused skill surface
15. **Zitadel** — Ory alternative, overlapping audience
16. **Caddy** — complements Traefik
17. **Ory/Hydra, Keto, Oathkeeper** — after Kratos lands
18. **Argo Workflows** — K8s-native container orchestration (infrastructure category)
19. **SeaweedFS** — Go-native distributed storage; new Storage domain, S3-compatible surface
