# Ruste

Ruste is a curated marketplace of AI agent skills and MCP integrations for [GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/copilot-cli/about-cli-plugins), [OpenAI Codex CLI](https://developers.openai.com/codex/), and [Claude Code CLI](https://code.claude.com/docs/en/discover-plugins). It brings developer workflows and experimental skills into one catalog.

## Getting started

Choose your client. You need its CLI installed and authenticated, plus access to this repository. Plugin-specific prerequisites are documented in each plugin's README.

### GitHub Copilot CLI

From this checkout, register the local marketplace:

```bash
copilot plugin marketplace add .
```

For a published copy, use `copilot plugin marketplace add Danta1st/ruste` instead.

```bash
copilot plugin marketplace browse ruste
copilot plugin install core@ruste
```

### OpenAI Codex CLI

From this checkout, register the local marketplace:

```bash
codex plugin marketplace add .
```

For a published copy, use `codex plugin marketplace add Danta1st/ruste` instead.

```bash
codex plugin list --marketplace ruste --available --json
codex plugin add core@ruste
```

### Claude Code CLI

From this checkout, register the local marketplace:

```bash
claude plugin marketplace add .
```

For a published copy, use `claude plugin marketplace add Danta1st/ruste` instead.

```bash
claude plugin install core@ruste
claude plugin list
```

Start a new Claude Code session after installation. To browse available plugins, run `/plugin` in that session and open the Discover tab.

The marketplace identifier is `ruste`. Replace `core` with another plugin from the catalog to install it.

## Available plugins

| Plugin | Purpose | Status |
|---|---|---|
| [`core`](./plugins/core/README.md) | Developer workflow skills and Context7 documentation lookup | Maintained workflow bundle |
| [`experimental`](./plugins/experimental/README.md) | Fact checking and repository knowledge maintenance | Candidate skills under evaluation |

## Using skills

Skills use the portable [Agent Skills](https://agentskills.io/) format. Clients can select a skill when your request matches its description. Invoke a skill explicitly with `/skill-name` in Copilot, `$skill-name` in Codex, or `/plugin-name:skill-name` in Claude Code:

| Task | GitHub Copilot CLI | OpenAI Codex CLI | Claude Code CLI |
|---|---|---|---|
| Prepare a standup | `/my-daily-summary Prepare my standup` | `$my-daily-summary Prepare my standup` | `/core:my-daily-summary Prepare my standup` |
| Review documentation | `/technical-writing Review this README` | `$technical-writing Review this README` | `/core:technical-writing Review this README` |

See the [Copilot skills documentation](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-skills), [Codex skills documentation](https://developers.openai.com/codex/skills), and [Claude Code skills documentation](https://code.claude.com/docs/en/skills#choose-where-skills-load) for client behavior.

## Managing plugins

### GitHub Copilot CLI

```bash
copilot plugin list
copilot plugin marketplace update ruste
copilot plugin update core
copilot plugin uninstall core
copilot plugin marketplace remove ruste
```

Uninstall plugins from a marketplace before removing its registration.

### OpenAI Codex CLI

```bash
codex plugin list
codex plugin marketplace upgrade ruste
codex plugin add core@ruste
codex plugin remove core@ruste
codex plugin marketplace remove ruste
```

For a Git-backed Codex marketplace, refresh its snapshot and reinstall the plugin to pick up changes. Local marketplace installations also use a cached plugin copy; reinstall after editing the source.

### Claude Code CLI

```bash
claude plugin list
claude plugin marketplace update ruste
claude plugin update core@ruste
claude plugin uninstall core@ruste
claude plugin marketplace remove ruste
```

Restart Claude Code after updating a plugin so the new copy loads.

### Moving an existing installation to Ruste

Existing registrations and installed plugin copies keep their previous marketplace identity. Register this checkout or `Danta1st/ruste`, then install the plugins you use with the `@ruste` suffix. Remove superseded plugin installations before unregistering the previous marketplace to avoid duplicate skills.

The `core` plugin no longer includes `my-sprint-summary`, `provider-scaffold`, onboarding components, or the bundled Azure DevOps MCP server. See the [core migration notes](./plugins/core/README.md#migration).

## Repository layout

```text
ruste/
├── .claude-plugin/marketplace.json
├── plugins/
│   ├── core/
│   └── experimental/
├── AGENTS.md
├── CLAUDE.md
├── CONTRIBUTING.md
└── README.md
```

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for plugin structure, documentation, versioning, and attribution requirements. The local `inbox/` directory holds research and evaluation assets and is ignored by Git.
