# flipt

A Claude Code plugin for managing [Flipt](https://flipt.io) feature flags — list, inspect, create,
enable/disable, delete, manage variants, and configure rollout rules and segments for controlled
rollouts. Bundles a Node.js helper that calls the Flipt REST API.

## What's included

A single `flipt` skill with a bundled CLI helper (`skills/flipt/flipt.mjs`):

- **Boolean & variant flags** — create, get, list, enable/disable, delete
- **Variants** — add/remove variants on a flag
- **Rollouts & segments** — threshold (percentage) and segment-based rollout rules
- **Output** — human-readable or `--json` for scripting

## Setup

The helper needs reach a Flipt instance and an API token. Copy and edit the env file:

```bash
cp skills/flipt/.env.example skills/flipt/.env
# set FLIPT_URL (default http://localhost:8080) and FLIPT_API_TOKEN
```

Then:

```bash
node skills/flipt/flipt.mjs list
node skills/flipt/flipt.mjs create my-feature -d "New feature"
node skills/flipt/flipt.mjs enable my-feature
```

## Alternative: official MCP server

For MCP-native setups, pair this skill with the official
[`flipt-io/mcp-server-flipt`](https://github.com/flipt-io/mcp-server-flipt) (Apache-2.0), which
exposes the same flag/segment/rule/rollout surface directly to Claude Code.

## Attribution

Ported from the Civitai Flipt skill in [`civitai/civitai`](https://github.com/civitai/civitai)
(Apache-2.0), genericized for a non-Civitai Flipt instance. See [NOTICE](NOTICE) for the exact
changes and [LICENSE](LICENSE) for terms.
