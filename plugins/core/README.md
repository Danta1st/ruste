# Ruste Core

Developer workflow skills and Context7 documentation lookup for GitHub Copilot CLI, OpenAI Codex CLI, and Claude Code CLI. This is Ruste's maintained workflow bundle.

## Installation

Register Ruste using the [marketplace setup guide](../../README.md#getting-started), then install `core`:

```bash
# GitHub Copilot CLI
copilot plugin install core@ruste

# OpenAI Codex CLI
codex plugin add core@ruste

# Claude Code CLI
claude plugin install core@ruste
```

## Components

The [plugin manifest](./.claude-plugin/plugin.json) owns plugin metadata and versioning.

| Skill | Purpose |
|---|---|
| [`my-daily-summary`](./skills/my-daily-summary/SKILL.md) | Prepare a standup from GitHub activity and supplied work |
| [`commit`](./skills/commit/SKILL.md) | Review staging and create Conventional Commits |
| [`create-pr`](./skills/create-pr/SKILL.md) | Prepare pull requests with useful descriptions and issue references |
| [`context7-mcp`](./skills/context7-mcp/SKILL.md) | Look up current library documentation |
| [`technical-writing`](./skills/technical-writing/SKILL.md) | Write and review technical documentation |
| [`domain-driven-design`](./skills/domain-driven-design/SKILL.md) | Model domains with consistent language, types, and diagrams |
| [`writing-tests`](./skills/writing-tests/SKILL.md) | Express behavioral requirements as readable automated tests |

The [MCP configuration](./.mcp.json) configures the bundled Context7 server.

Supporting examples include a [standup](./skills/my-daily-summary/examples/standup-example.md). Each skill links to any additional references it needs.

## Usage

Use `/skill-name` in Copilot, `$skill-name` in Codex, or `/core:skill-name` in Claude Code:

```text
/my-daily-summary Prepare my standup
$technical-writing Review this README
/core:domain-driven-design Review the order lifecycle
```

## Prerequisites and configuration

| Capability | Requirements |
|---|---|
| Plugin installation | A supported client with plugin support; see [shared setup](../../README.md#getting-started) |
| GitHub summaries and pull requests | [GitHub CLI](https://cli.github.com/) authenticated for the repositories you use |
| Commits | Git and an existing repository; the skill checks repository and branch conventions |
| Context7 | Node.js and `npx`, network access, and remaining Context7 service quota |

### Context7 MCP server

The [MCP configuration](./.mcp.json) launches `@upstash/context7-mcp@latest` with `npx` and exposes documentation lookup tools. It contains no organization defaults, environment variables, or credentials. Service quotas and any account requirements are controlled by [Context7](https://github.com/upstash/context7#readme).

The package is resolved at launch rather than pinned in this repository. See the upstream documentation for current runtime requirements and authentication options.

## Compatibility and limitations

Skills use the portable Agent Skills format. Skill selection and MCP loading follow each client's capabilities; see [shared client guidance](../../README.md#using-skills). External activity summaries depend on service access and available evidence. Missing access must be reported as incomplete coverage.

## Migration

The Azure DevOps MCP server and `my-sprint-summary` have been removed from the bundle. Context7 remains bundled.

`provider-scaffold` and its supporting script, templates, and example have been removed. The `/onboarding` command and `onboarding-guide` agent have also been removed. These components are no longer available through `core`.

Reinstall `core@ruste` after refreshing your marketplace so the installed copy reflects these changes. See [marketplace management](../../README.md#managing-plugins) for client-specific commands.
