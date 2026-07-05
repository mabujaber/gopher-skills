# openfga

A Claude Code plugin for designing, reviewing, and refactoring [OpenFGA](https://openfga.dev)
authorization models — types, relations, relationship tuples, `can_*` permissions, usersets,
and `.fga.yaml` tests — across JavaScript/TypeScript, Go, Python, Java, and .NET.

## What's included

- **Authorization model design** — types, relations, permission structures, separation of model vs. data.
- **Relationship patterns** — direct, indirect (`X from Y`), concentric, usersets, conditions, wildcards.
- **Custom roles** — simple static, resource-specific, and combined role patterns.
- **Optimization** — model simplification, tuple minimization, type restrictions.
- **Testing & validation** — `.fga.yaml` structure, `check`/`list_objects`/`list_users` assertions, CLI usage.
- **SDK integration** — JavaScript/TypeScript, Go, Python, Java, .NET.

## When it activates

The skill triggers when working with `.fga` model files, `.fga.yaml` test files, OpenFGA
relationship definitions, permission structures, or authorization logic, including SDK integrations.

## Attribution

Ported from the official [`openfga/agent-skills`](https://github.com/openfga/agent-skills)
project (Apache-2.0). See [NOTICE](NOTICE) for provenance and [LICENSE](LICENSE) for terms.
