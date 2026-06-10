# Roadmap

Planned plugins for gopher-skills, organized by domain.

## Workflows

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `temporal-developer` | [Temporal](https://temporal.io) | Durable execution platform | Python, TypeScript, Go, Java |

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
| `openfga` | [OpenFGA](https://openfga.dev) | Fine-grained authorization (CNCF) | Go, Node.js, .NET, Python, Java |
| `zitadel` | [Zitadel](https://zitadel.com) | Cloud-native identity platform | Go, REST/gRPC (any language) |

## Messaging & Event Streaming

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `nats` | [NATS](https://nats.io) | High-performance messaging & streaming | Go, Python, JS, Java, .NET, Rust, 40+ |

## Observability

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `signoz` | [SigNoz](https://signoz.io) | OpenTelemetry-native observability platform | OpenTelemetry SDKs (all languages) |

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
| `k6` | [k6](https://k6.io) | Developer-centric load testing tool | JavaScript test scripts, Go extensions |

## Feature Flags

| Plugin | Tool | Description | SDKs |
|--------|------|-------------|------|
| `flipt` | [Flipt](https://flipt.io) | Self-hosted feature flag platform | Go, Python, JS, Ruby, Java, .NET, REST |

## Status Legend

| Status | Meaning |
|--------|---------|
| ✅ Available | Plugin is complete and installable |
| 🚧 In Progress | Actively being built |
| 📋 Planned | On the roadmap, not started |

## Priority Order

1. **NATS** — broadest language coverage (40+ SDKs), huge community
2. **Ory/Kratos** — most-used Ory product, clear developer skill surface
3. **Traefik** — widely deployed, complex config surface benefits from a skill
4. **OpenFGA** — growing fast with AI authorization use cases
5. **k6** — every team needs load testing
6. **SigNoz** — OpenTelemetry-native, fills observability gap
7. **Flipt** — self-hosted feature flags, strong SDK coverage
8. **imgproxy** — niche but highly focused skill surface
9. **Zitadel** — Ory alternative, overlapping audience
10. **Caddy** — complements Traefik
11. **Ory/Hydra, Keto, Oathkeeper** — after Kratos lands
12. **Argo Workflows** — K8s-native container orchestration (infrastructure category)
