# Agent Instructions — Ruste

## Marketplace identity

Use **Ruste** in prose, `ruste` as the marketplace identifier, and `Danta1st/ruste` as the GitHub repository. Keep integrations organization-neutral. Preserve third-party attribution when adapting imported assets; anonymize organization-specific evaluation data and label it as adapted history.

## Versioning

`plugins/<plugin-name>/.claude-plugin/plugin.json` is the **only** place a plugin version lives.

| Location | Rule |
|----------|------|
| `plugin.json` `"version"` | ✅ Update here — and only here |
| `marketplace.json` `plugins[]` | ❌ No version field |
| `SKILL.md` / command / agent frontmatter | ❌ No version field — not supported by the CLI |

Bump `plugin.json` in the same commit as the change. Use semver: patch for fixes, minor for new components, major for renames/removals.

## Skill Frontmatter

Only these fields are supported — do not add others:

```yaml
---
name: skill-name
description: "..."
license: Apache 2.0   # optional
---
```

## Naming

Personal/self-service skills use a `my-` prefix (e.g. `my-daily-summary`). Team-scoped skills use plain names.

## Plugin Documentation

Follow the [plugin documentation policy](./CONTRIBUTING.md#documentation-policy) for every change under `plugins/`.

Before editing:

1. Read `plugins/<plugin-name>/README.md`, the manifest, and the public components being changed. For a new plugin or a missing README, plan to create the README in the same change.
2. Identify affected behavior, usage, links, examples, prerequisites, configuration, compatibility, operations, migrations, and repository-level references.

During editing:

1. Update the plugin README in the same change whenever a user-facing contract changes.
2. Update the root plugin catalog when adding, renaming, deprecating, or removing a plugin.
3. Keep executable files and tests as the behavioral source of truth. Link to component files instead of duplicating their contents.
4. Do not add version metadata outside `plugins/<plugin-name>/.claude-plugin/plugin.json`.

After editing:

1. Verify local documentation links, examples, names, and terminology against the changed files.
2. Verify the required semantic version bump and relevant tests or checks.
3. Report either `Documentation updated: <paths>` or `Documentation not required: <reviewed paths and concrete reason>` in the pull request or final handoff.

Do not treat a legacy documentation gap as permission to expand unrelated work. For a changed noncompliant plugin, document the affected behavior now and identify remaining gaps for a follow-up issue. Do not consider the plugin change complete while its documentation evidence or required updates are missing.
