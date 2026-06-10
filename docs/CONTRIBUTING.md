# Contributing to gopher-skills

## Adding a Plugin

### 1. Choose the right location

Check [ROADMAP.md](../ROADMAP.md) for the domain and path. Follow the structure:

- Single tool: `plugins/<domain>/<plugin-name>/`
- Vendor with multiple products: `plugins/<domain>/<vendor>/<product>/`

Example:
```
plugins/identity/zitadel/          ← single tool
plugins/identity/ory/kratos/       ← vendor namespace
```

### 2. Scaffold from the template

```bash
cp -r templates/plugin plugins/<domain>/<plugin-name>
```

### 3. Fill in plugin.json

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "Short description",
  "author": { "name": "Tool Author", "url": "https://tool.io" },
  "repository": "https://github.com/mabujaber/gopher-skills",
  "license": "MIT or tool's license",
  "keywords": ["go-ecosystem", "tool-name"]
}
```

### 4. Write the SKILL.md

Required frontmatter:
```yaml
---
name: skill-name
description: >
  One or two sentences describing what this skill does.
  Include trigger phrases so Claude activates it at the right time.
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(tool:*)
version: 1.0.0
license: MIT
tags:
  - go-ecosystem
  - your-tool
compatibility: Designed for Claude Code.
---
```

Skill body should cover:
- Overview of the tool
- Getting started / installation check
- Primary reference files to read
- How Claude should use the skill

### 5. Add reference docs

Organize under `skills/<skill-name>/references/`:
- `core/` — language-agnostic concepts
- `<language>/` — per-language implementation guides

Keep files focused: one topic per file. Claude reads them lazily at runtime.

### 6. Register in marketplace.json

Add an entry to `.claude-plugin/marketplace.json`:
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

Also update `metadata.totalPlugins` and `metadata.skillsEnabled` counts.

### 7. Update README.md and ROADMAP.md

- Add a row to the plugin table in `README.md`
- Update the status in `ROADMAP.md` from 📋 Planned → ✅ Available

## Porting from Codex / OpenAI Plugins

When porting from `openai/plugins`:

1. Copy the `references/` directory verbatim — no changes needed
2. Adapt `SKILL.md`:
   - Add Claude Code frontmatter (`allowed-tools`, `tags`, `compatibility`)
   - Remove Slack/community feedback prompts
   - Keep all reference links as-is
3. Drop `agents/openai.yaml` — not used by Claude Code
4. Add `.claude-plugin/plugin.json` and `package.json`

## Domain Categories

| Domain | When to use |
|--------|-------------|
| `workflows/` | Durable execution, job orchestration, DAGs |
| `identity/` | Auth, OAuth2, SSO, permissions, authorization |
| `messaging/` | Message brokers, pub/sub, event streaming |
| `observability/` | Metrics, traces, logs, dashboards |
| `gateway/` | Reverse proxies, API gateways, load balancers |
| `media/` | Image/video/file processing |
| `testing/` | Load testing, performance, chaos |
| `feature-flags/` | Feature flag platforms and SDKs |
