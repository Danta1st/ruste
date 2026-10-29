# OKF v0.2 behavior

Use this reference to discover, parse, initialize, or validate an OKF bundle.
The [canonical OKF v0.2 specification](https://github.com/GoogleCloudPlatform/open-knowledge-format/blob/main/SPEC.md)
is normative. Recheck it before changing supported OKF syntax or semantics; do
not require a network call for ordinary retrieval under this pinned profile.

## Base OKF conformance

- A bundle is a directory tree of UTF-8 Markdown files.
- `index.md` and `log.md` are reserved at every level and are not concepts.
- Every other Markdown file is a concept with parseable YAML frontmatter and a
  non-empty `type`. No body section is required by base OKF.
- A concept ID is its bundle-relative path with the `.md` suffix removed.
  Moving a concept therefore changes its ID.
- `type` is the only always-required concept field. Treat unknown types as
  generic concepts. Accept and preserve producer-defined fields when editing.
- Do not reject a bundle for missing optional fields or indexes, unknown fields
  or types, or broken links. Report a broken link when it matters to the task.

Recommended concept fields are `title`, `description`, `resource`, and `tags`.
The following optional standard fields retain their OKF meanings:

- `sources` records materials from which the concept derives. Each entry has a
  required `resource`; other source signals are optional.
- `generated` records who or what produced the current content. When present,
  it is a mapping with required `by` and optional `at`.
- `verified` records independent confirmation. Each event has required `by` and
  `at`. It may be one mapping or a list; consume one mapping as a one-element
  list.
- `status` is `draft`, `stable`, or `deprecated`. An absent status means
  `stable` under base OKF.
- `stale_after` is an absolute ISO 8601 instant with an explicit UTC offset. At
  or after that instant, treat the concept as stale.

Trust signals are advisory, not access control. Timestamps in standard OKF
timestamp fields use ISO 8601 datetimes with explicit UTC offsets. Actor values
use `<producer>/<version>` for agents or tools, `human:<id>` for people, and
`process:<id>` for automated processes.

## Navigation and relationships

Use standard Markdown links for relationships. A leading `/` is relative to
the bundle root; normal relative links are also valid. Relationship meaning
comes from the surrounding prose.

An `index.md` progressively discloses a directory's contents. Its entries should
include the linked concept's description. Indexes normally have no frontmatter;
only the bundle-root index may declare `okf_version` in frontmatter. `log.md` is
optional update history, not required by this skill.
Resolve a literal index target against the filesystem before reporting it as
missing or broken.

## Repository profile

The following rules are stricter than base OKF and apply only when creating or
maintaining a bundle under this skill's default profile:

- A new root index declares `okf_version: "0.2"`.
- New bundles use `docs/knowledge/<area>/` unless repository instructions select
  another location. Preserve an existing conforming root and profile.
- Directories represent stable bounded contexts, services, business
  capabilities, or repository areas—not types, languages, or frameworks.
- Active indexes list stable concepts and copy each concept's frontmatter
  `description` verbatim. Draft and deprecated concepts stay out of active
  lists unless lifecycle history is the index's explicit purpose.
- New concepts use the smallest profile-appropriate subset of `type`, `title`,
  `description`, `status`, `tags`, `applies_to`, `resource`, `sources`, and
  when meaningful `generated`, `verified`, and `stale_after`. Omit empty
  optional fields.
- `applies_to` is a producer-defined retrieval list. Values are literal
  repository-relative paths or prefixes, project or package names, services,
  modules, or named external boundaries. They are not globs or regular
  expressions.
- Use `generated`, `verified`, or `stale_after` only when they change trust or
  behavior. Do not add timestamps to immutable Decisions for completeness.
- Update concepts and every affected index or link in the same change. Moving a
  concept requires an explicit migration plan because its ID changes.
