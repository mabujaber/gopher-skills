# gopher-skills

Claude Code plugins and skills for the Go-native ecosystem.

## Repository Layout

```
plugins/
  <domain>/
    <plugin-name>/           Standard plugin (one tool)
      .claude-plugin/
        plugin.json          Plugin metadata
      skills/<skill-name>/
        SKILL.md             Skill (frontmatter + instructions)
        references/          Reference docs loaded at runtime
      commands/              Slash command entry points
      README.md
      package.json
      LICENSE
    <vendor>/                Vendor namespace (multiple related tools)
      <product>/             Each product is its own plugin
        .claude-plugin/
        skills/
        ...
```

## Domain Categories

| Domain | Path | Tools |
|--------|------|-------|
| Languages | `plugins/languages/` | golang-pro |
| Workflows | `plugins/workflows/` | Temporal |
| Infrastructure | `plugins/infrastructure/` | Argo Workflows |
| Identity | `plugins/identity/` | `ory/` (Kratos, Hydra, Keto, Oathkeeper), OpenFGA, Zitadel |
| Messaging | `plugins/messaging/` | NATS |
| Observability | `plugins/observability/` | SigNoz |
| Gateway | `plugins/gateway/` | Traefik, Caddy |
| Media | `plugins/media/` | imgproxy |
| Testing | `plugins/testing/` | k6 |
| Feature Flags | `plugins/feature-flags/` | Flipt |

## Vendor Namespacing

When a vendor has multiple related products, nest them under a vendor directory:
```
plugins/identity/ory/kratos/
plugins/identity/ory/hydra/
plugins/identity/ory/keto/
plugins/identity/ory/oathkeeper/
```
This keeps related tools grouped and makes the category scannable.

## Adding a New Plugin

1. Copy `templates/plugin/` to `plugins/<domain>/<plugin-name>/`
2. Edit `.claude-plugin/plugin.json`
3. Write `skills/<skill-name>/SKILL.md` with Claude Code frontmatter
4. Add reference docs under `skills/<skill-name>/references/`
5. Register the plugin in `.claude-plugin/marketplace.json`
6. Update `README.md` plugin table and `ROADMAP.md` status

## SKILL.md Frontmatter Requirements

```yaml
---
name: <skill-name>
description: >
  One or two sentences. Include trigger phrases.
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(<namespace>:*)
version: 1.0.0
license: <license>
tags:
  - go-ecosystem
  - <tool-name>
compatibility: Designed for Claude Code.
---
```

## marketplace.json

Located at `.claude-plugin/marketplace.json`. Add an entry for every new plugin:
```json
{
  "name": "plugin-name",
  "source": "./plugins/<domain>/<plugin-name>",
  "description": "...",
  "version": "1.0.0",
  "category": "<domain>",
  "keywords": [...],
  "author": { "name": "...", "url": "..." }
}
```

## Porting from Other Plugin Ecosystems

When porting from `openai/plugins` (Codex):
- Drop `agents/openai.yaml`
- Add `.claude-plugin/plugin.json` and `package.json`
- Add Claude Code frontmatter to `SKILL.md`
- Remove OpenAI/Codex-specific branding
