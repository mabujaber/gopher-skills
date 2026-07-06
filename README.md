# gopher-skills

Claude Code plugins and skills for the **Go-native ecosystem** — tools built with Go that are used across languages.

> Whether you write Go, Python, TypeScript, Java, .NET, or anything else —
> if your stack runs on Go-built infrastructure, this is your plugin library.

## Available Plugins

| Plugin | Category | Languages | Status |
|--------|----------|-----------|--------|
| [temporal-developer](plugins/workflows/temporal-developer/) | workflows | Python, TypeScript, Go, Java | ✅ Available |
| [golang-pro](plugins/languages/golang-pro/) | language | Go | ✅ Available |
| [nats](plugins/messaging/nats/) | messaging | Go, + 40 client libraries | ✅ Available |

## Roadmap

See [ROADMAP.md](ROADMAP.md) for the full list of planned plugins.

## Plugin Categories

```
plugins/
  languages/          Go language development — idiomatic patterns, testing, concurrency
    golang-pro/
  workflows/          Durable execution & application-layer orchestration
    temporal-developer/
  infrastructure/     Kubernetes-native & platform tooling
    argo-workflows/         (planned)
  identity/           Authentication, authorization & access control
    ory/                    (planned)
      kratos/               User identity & auth
      hydra/                OAuth2 / OIDC server
      keto/                 Permissions & access control
      oathkeeper/           Identity & access proxy
    openfga/                (planned) Fine-grained authorization
    zitadel/                (planned) Cloud-native identity platform
  messaging/          Messaging & event streaming
    nats/                   High-performance messaging & streaming (JetStream)
  observability/      Metrics, tracing & logs
    signoz/                 (planned)
  gateway/            Reverse proxies & API gateways
    traefik/                (planned)
    caddy/                  (planned)
  media/              Image & media processing
    imgproxy/               (planned)
  testing/            Load & performance testing
    k6/                     (planned)
  feature-flags/      Feature flag management
    flipt/                  (planned)
  storage/            Distributed object & blob stores
    seaweedfs/              (planned) S3-compatible distributed object store
```

## Installation

```bash
# Add gopher-skills as a Claude Code marketplace
claude plugin marketplace add mabujaber/gopher-skills

# Install a plugin
claude plugin install temporal-developer@gopher-skills
```

## Contributing

See [docs/CONTRIBUTING.md](docs/CONTRIBUTING.md) for how to add a new plugin.

## License

MIT
