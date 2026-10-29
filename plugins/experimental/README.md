# Ruste Experimental

An inbox for experimental skills. These skills may need additional real-world testing and review before promotion to a stable plugin.

## Installation

Register Ruste using the [marketplace setup guide](../../README.md#getting-started), then install the plugin:

```bash
# GitHub Copilot CLI
copilot plugin install experimental@ruste

# OpenAI Codex CLI
codex plugin add experimental@ruste

# Claude Code CLI
claude plugin install experimental@ruste
```

## Skills

| Skill | Purpose | Client configuration |
|---|---|---|
| [`fact-check`](./skills/fact-check/SKILL.md) | Assess externally verifiable claims against available evidence. | [OpenAI policy](./skills/fact-check/agents/openai.yaml) |
| [`maintain-okf-wiki`](./skills/maintain-okf-wiki/SKILL.md) | Retrieve, onboard, and maintain repository-local OKF knowledge. | [OpenAI interface](./skills/maintain-okf-wiki/agents/openai.yaml) |

See the [plugin manifest](./.claude-plugin/plugin.json) for plugin metadata.

## Usage

Invoke a skill explicitly with `/skill-name` in GitHub Copilot CLI, `$skill-name` in OpenAI Codex CLI, or `/experimental:skill-name` in Claude Code:

```text
$fact-check Check these claims: ...
$maintain-okf-wiki onboard analyze
/experimental:fact-check Check these claims: ...
/experimental:maintain-okf-wiki onboard analyze
```

`fact-check` does not guarantee that a claim is true or complete. Its conclusions depend on available evidence and live research access. `maintain-okf-wiki` requires approval before changing repository knowledge.

`maintain-okf-wiki` prefers indexes for retrieval and creates indexed bundles when initialization is approved. Existing OKF bundles may omit root or area indexes: the skill uses concept frontmatter and targeted body inspection to build navigation in memory, preserving the bundle. Discovery stays within task-relevant documentation areas; ambiguous bundle roots are surfaced before deriving concept IDs or resolving bundle-root links.

## Requirements and compatibility

The plugin supports GitHub Copilot CLI, OpenAI Codex CLI, and Claude Code CLI. It has no plugin-level environment variables, authentication requirements, or external services. Individual skills may require file access or live research to complete a request.

Treat every skill as experimental until it is promoted to a stable plugin.

## Migration

Install `experimental@ruste` after registering the Ruste marketplace. The skills listed above are the components available in this copy; no pending promotions to other plugins are assumed.
