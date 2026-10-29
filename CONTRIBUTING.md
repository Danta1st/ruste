# Contributing to Ruste

Ruste is a curated AI agent plugin marketplace for GitHub Copilot CLI, OpenAI Codex CLI, and Claude Code CLI. This guide explains how to add new plugins or update existing ones.

Use **Ruste** for the display name, `ruste` for the marketplace identifier, and `Danta1st/ruste` for the GitHub repository. Keep team-specific URLs, contacts, package scopes, and service defaults out of reusable components. Preserve upstream authorship, source links, and applicable license notices for imported skills; Ruste curation does not transfer their authorship or change their license.

## Client compatibility

Keep reusable workflows in the portable [Agent Skills](https://agentskills.io/) format:

```text
skills/<skill-name>/SKILL.md
```

Each `SKILL.md` must contain `name` and `description` frontmatter. Write skill instructions so they work across clients. Keep client-specific commands, agents, and configuration in their respective plugin locations.

Claude Code invokes plugin skills as `/plugin-name:skill-name`; Copilot uses `/skill-name`, and Codex uses `$skill-name`. Include the relevant forms in usage examples. See the [shared client guidance](./README.md#using-skills).

Shared repository instructions live in [AGENTS.md](./AGENTS.md). [CLAUDE.md](./CLAUDE.md) imports that file so Claude Code loads the same rules, including on versions that do not read `AGENTS.md` directly. Keep shared rules in `AGENTS.md`; see [Claude Code instruction imports](https://code.claude.com/docs/en/memory#import-additional-files).

## Documentation policy

The canonical documentation for a plugin is `plugins/<plugin-name>/README.md`. The repository-root `README.md` must contain only marketplace-wide installation, the plugin catalog, and shared client guidance. `CONTRIBUTING.md` owns contribution and maintenance policy. Do not duplicate those subjects in every plugin README; link to the repository-level source instead.

Each plugin README must contain, using headings appropriate to the plugin:

- its purpose, intended users, and stability or lifecycle status;
- installation and the smallest working usage example;
- a component index that links to the plugin manifest, every public skill, command, agent, and configuration file, and any supporting examples;
- prerequisites, configuration, authentication requirements, and compatibility constraints, or an explicit statement that none apply; and
- limitations, migrations, deprecations, and operational notes when they affect users.

Link to manifests and component source files instead of copying their full contents. Executable files and tests define behavior. The plugin README records intent, usage, and user-facing contracts. When documentation disagrees with executable behavior, fix the documentation or the behavior in the same change; do not leave both interpretations in place. A plugin version remains authoritative only in `plugins/<plugin-name>/.claude-plugin/plugin.json`.

### Documentation impact by change

| Plugin change | Documentation that must be reviewed |
|---|---|
| Add a plugin or public component | Add or update the plugin README component index, usage, prerequisites, and example; update the root plugin catalog for a new plugin. |
| Change behavior, inputs, outputs, triggers, configuration, authentication, dependencies, compatibility, or operations | Update every affected README section, example, migration note, and linked component-specific reference. |
| Rename or move a plugin or component | Update links and examples in the plugin README and all repository-level references; provide migration guidance when users must act. |
| Deprecate or remove a plugin or component | Mark the deprecation and replacement before removal when practical; on removal, delete or update stale links, examples, catalog entries, and migration guidance. |
| Fix internal implementation without changing a user-facing contract | Review the plugin README. A documentation edit is not required if the pull request records the reviewed paths and explains why behavior and usage are unchanged. |
| Bump a plugin version | Review documentation for the underlying change. Do not copy the version into documentation unless a compatibility or migration note requires it. |

The author of a plugin change owns its documentation impact. Before editing, identify affected public behavior and documentation paths. During the change, update documentation with the executable files rather than in a later pull request. After editing, verify links, examples, terminology, the manifest version bump, and the relevant behavior or tests.

Every pull request that changes `plugins/` must include documentation evidence in its description:

- `Documentation updated:` followed by the changed documentation paths; or
- `Documentation not required:` followed by the README and other paths reviewed and a concrete reason that no user-facing contract changed.

Reviewers must block a plugin change when this evidence is absent, a required README is missing, documented components or links are stale, or user-facing behavior changed without matching documentation. This review checklist is the current enforcement control; the repository has no automated documentation validator. As a narrowly scoped follow-up, add a CI validator that fails when a marketplace plugin lacks `plugins/<plugin-name>/README.md`, a documented local link is broken, or a plugin change provides neither updated documentation nor the required pull-request evidence.

Existing plugins may be brought into compliance incrementally. Unrelated changes must not be blocked solely by a legacy documentation gap. The first pull request that changes a noncompliant plugin must document the affected behavior and record any remaining gaps in a follow-up issue; additions, renames, deprecations, removals, and migrations introduced by that pull request must comply immediately. Work is complete when executable behavior, plugin documentation, repository-level references, versioning, verification, and pull-request evidence agree.

---

## Adding a New Plugin

### 1. Create the plugin directory

Each plugin lives in its own subdirectory under `plugins/`:

```bash
mkdir -p plugins/your-plugin-name
```

Use lowercase kebab-case for the directory name (e.g., `dotnet-helpers`, `deploy-workflow`).

### 2. Add the required plugin manifest

Create `plugins/your-plugin-name/.claude-plugin/plugin.json`:

```json
{
  "name": "your-plugin-name",
  "version": "0.1.0",
  "description": "A short description of what this plugin does.",
  "author": {
    "name": "Ruste"
  }
}
```

### 3. Add plugin components

Depending on what the plugin does, add one or more of:

| Component | Directory | Purpose |
|---|---|---|
| Skills | `skills/<skill-name>/SKILL.md` | Knowledge that auto-activates when relevant |
| Commands | `commands/<command-name>.md` | Slash commands users invoke manually |
| Agents | `agents/<agent-name>.md` | Autonomous agents for specific tasks |
| MCP servers | `.mcp.json` | External service integrations |

Use the existing `plugins/core/` plugin as a reference for structure and file format.

### 4. Add a README

Create the canonical `plugins/your-plugin-name/README.md` and follow the [documentation policy](#documentation-policy).

### 5. Register in the marketplace

Open `.claude-plugin/marketplace.json` and add your plugin to the `plugins` array. This marketplace is used by GitHub Copilot CLI, OpenAI Codex CLI, and Claude Code CLI:

```json
{
  "name": "your-plugin-name",
  "description": "A short description of what this plugin does.",
  "source": "./plugins/your-plugin-name"
}
```

### 6. Submit your changes

```bash
git checkout -b add-your-plugin-name
git add .
git commit -m "feat(plugins): add your-plugin-name"
git push origin add-your-plugin-name
# Open a pull request for review
```

---

## Updating an Existing Plugin

1. Make your changes in `plugins/<plugin-name>/`
2. Review and update `plugins/<plugin-name>/README.md` and any affected repository-level documentation.
3. Bump the `version` field in `plugins/<plugin-name>/.claude-plugin/plugin.json`.
4. Record documentation evidence in the pull request description.
5. Commit and push.
6. Refresh the plugin:

   ```bash
   # GitHub Copilot CLI
   copilot plugin update <plugin-name>

   # OpenAI Codex CLI
   codex plugin marketplace upgrade ruste
   codex plugin add <plugin-name>@ruste

   # Claude Code CLI
   claude plugin marketplace update ruste
   claude plugin update <plugin-name>@ruste
   ```

   Restart Claude Code after updating the plugin.

---

## Plugin Versioning

Use [semantic versioning](https://semver.org/):

- `0.x.x` — Initial development, breaking changes may occur
- `1.0.0` — Stable, ready for general team use
- Patch (`x.x.1`) — Bug fixes and small content updates
- Minor (`x.1.x`) — New skills, commands, or agents added (backwards compatible)
- Major (`1.x.x`) — Breaking changes (e.g., renamed commands or removed features)

---

## Questions?

Open an issue in the [Ruste repository](https://github.com/Danta1st/ruste/issues).
