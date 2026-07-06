---
name: flipt
description: >
  Use when managing Flipt feature flags — listing, creating, enabling/disabling, deleting flags,
  adding/removing variants, and configuring rollout rules and segments for controlled rollouts.
  Triggers on "flipt", "feature flag", "feature flags", "rollout", "variant flag", "segment",
  "enable flag", "disable flag", or "flag rollout".
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(node:*), Bash(gh:*), Bash(git:*)
version: 1.0.0
license: MIT
tags:
  - go-ecosystem
  - flipt
  - feature-flags
  - rollouts
  - experimentation
compatibility: Designed for Claude Code.
---

# Flipt Feature Flag Management

[Flipt](https://flipt.io) is a self-hosted, Go-native feature flag platform. This skill manages
Flipt flags over the Flipt REST API via a bundled helper script — list, inspect, create,
enable/disable, delete, manage variants, and configure rollout rules.

## Concepts

- **Flag** — a boolean or variant toggle, identified by a unique `key` within a namespace.
- **Variant flag** — a flag that returns one of several `variants` (e.g. `"v1"`, `"v2"`) instead of true/false.
- **Segment** — a reusable targeting rule (a set of constraints on flag evaluation context, e.g. "moderators").
- **Rollout rule** — attaches a variant/boolean value to a threshold (percentage) or a segment.
- **Namespace** — an isolated scope for flags (default: `default`).

Docs: https://docs.flipt.io

## Prerequisites

This skill needs reach a Flipt instance and an API token:

1. A running Flipt server (self-hosted `http://localhost:8080` by default, or Flipt Cloud).
2. `FLIPT_URL` and `FLIPT_API_TOKEN` set — either in the shell environment, a project `.env`,
   or the skill's own `.env` (copy `.env.example` → `.env` next to `flipt.mjs`).

```bash
cp skills/flipt/.env.example skills/flipt/.env
# then edit skills/flipt/.env with your FLIPT_URL and FLIPT_API_TOKEN
```

## Running Commands

```bash
node skills/flipt/flipt.mjs <command> [options]
```

### Commands

| Command | Description |
|---------|-------------|
| `list` | List all flags |
| `get <key>` | Get details for a specific flag |
| `create <key>` | Create a new flag (boolean by default) |
| `enable <key>` | Enable a flag (set to true) |
| `disable <key>` | Disable a flag (set to false) |
| `delete <key>` | Delete a flag (requires confirmation) |
| `add-variant <flag> <variant>` | Add a variant to an existing flag |
| `remove-variant <flag> <variant>` | Remove a variant from a flag |
| `set-rollout <flag> <variant>` | Set rollout rule for a variant |

### Options

| Flag | Description |
|------|-------------|
| `--description <text>`, `-d` | Description for new flag |
| `--enabled` | Create flag as enabled (default: disabled) |
| `--variant` | Create as variant flag (default: boolean) |
| `--variants <keys>` | Comma-separated variant keys (first is default) |
| `--default <key>` | Set default variant key |
| `--rollout <pct>` | Rollout percentage (default: 100) |
| `--segment <key>` | Segment key for rules (default: all-users) |
| `--json` | Output results as JSON |
| `--quiet`, `-q` | Minimal output |
| `--force`, `-f` | Skip confirmation prompts |

### Examples

```bash
# List all flags
node skills/flipt/flipt.mjs list

# Get a specific flag
node skills/flipt/flipt.mjs get my-feature

# Create a new flag (disabled by default)
node skills/flipt/flipt.mjs create my-new-feature -d "Enable new feature for testing"

# Create a flag that's enabled immediately
node skills/flipt/flipt.mjs create my-feature --enabled -d "Already enabled feature"

# Enable / disable a flag
node skills/flipt/flipt.mjs enable my-new-feature
node skills/flipt/flipt.mjs disable my-new-feature

# Delete a flag (with confirmation, or --force to skip)
node skills/flipt/flipt.mjs delete old-flag
node skills/flipt/flipt.mjs delete old-flag --force

# JSON output for scripting
node skills/flipt/flipt.mjs list --json
```

## GitOps Backend (optional)

If your Flipt uses the [GitOps backend](https://docs.flipt.io/configuration/storage#git), the Git
repository is the source of truth — changes made via the API are temporary and are overwritten on
the next sync. For **permanent** changes in that setup, edit the state repository directly:

```bash
# Clone your Flipt state repo
gh repo clone <your-org>/flipt-state /tmp/flipt-state

# Edit the flag definitions (e.g. <namespace>/features.yaml), then commit and push
cd /tmp/flipt-state
git add -A && git commit -m "Add new feature flag" && git push
```

Example flag definition:

```yaml
flags:
  - key: my-feature-flag
    name: my-feature-flag
    type: BOOLEAN_FLAG_TYPE
    description: Description of what this flag controls
    enabled: false
    rollouts:
      - threshold:
          percentage: 50
          value: true
      - segment:
          keys:
            - moderators
          operator: OR_SEGMENT_OPERATOR
          value: true
```

## Safety Notes

1. **API vs. GitOps** — if you use the GitOps backend, the Git repo is the source of truth; API changes are temporary.
2. **Test before enabling** — use segments for gradual rollout rather than flipping a flag on for everyone.
3. **Coordinate with your team** — others may be editing the same flags.

## Alternative: official Flipt MCP server

For MCP-native setups, pair this skill with the official
[`flipt-io/mcp-server-flipt`](https://github.com/flipt-io/mcp-server-flipt) (Apache-2.0), which
exposes flag/segment/rule/rollout/evaluation/namespace tools directly to Claude Code. This plugin
ships the script-based helper instead of the MCP runtime; both target the same Flipt API.
