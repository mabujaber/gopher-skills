# signoz

A Claude Code plugin for [SigNoz](https://signoz.io) — open-source, OpenTelemetry-native
observability (metrics, traces, and logs in one place). Official SigNoz skills, ported from
[`SigNoz/agent-skills`](https://github.com/SigNoz/agent-skills) (MIT).

## Setup

The skills operate on a live SigNoz instance through the **SigNoz MCP server**. After installing
the plugin, configure the endpoint via `SIGNOZ_MCP_URL` (defined in `plugin.json` `userConfig`):

- **SigNoz Cloud** — `https://mcp.<region>.signoz.cloud/mcp` (us, us2, eu, eu2, in, in2). Run `/mcp`,
  select `signoz`, and complete the OAuth login.
- **Self-hosted** — your own HTTP `/mcp` URL, e.g. `http://localhost:8000/mcp`.

See the `signoz-mcp-setup` skill for detailed client configuration.

## Skills (12)

| Skill | What it does |
|-------|--------------|
| `signoz-mcp-setup` | Connect Claude Code to the SigNoz MCP server |
| `signoz-setting-up-observability` | Instrument apps with OpenTelemetry for SigNoz |
| `signoz-generating-queries` | Run ad-hoc metrics / logs / traces / exceptions queries |
| `signoz-writing-clickhouse-queries` | Write ClickHouse queries over SigNoz data |
| `signoz-creating-dashboards` | Build SigNoz dashboards |
| `signoz-modifying-dashboards` | Modify existing dashboards |
| `signoz-explaining-dashboards` | Explain what a dashboard shows in operational terms |
| `signoz-creating-alerts` | Create alert rules from natural-language intent |
| `signoz-explaining-alerts` | Explain existing alert rules |
| `signoz-investigating-alerts` | Investigate a firing alert (baseline + neighbor signals) |
| `signoz-managing-views` | Manage saved Explorer views |
| `signoz-searching-docs` | Search the SigNoz documentation |

## Attribution

Ported from the official [`SigNoz/agent-skills`](https://github.com/SigNoz/agent-skills) plugin
(MIT). See [NOTICE](NOTICE) for the full file manifest and what was intentionally not ported, and
[LICENSE](LICENSE) for terms.
