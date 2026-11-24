# Local backlog protocol

Each file records one independently verifiable outcome. Item frontmatter is
authoritative. The index summarizes it for discovery and may also contain
hand-written planning decisions.

## Metadata

Use YAML frontmatter, one field per line, with quoted strings for titles,
targets, paths, and tags and inline lists for collections.

| Field | Meaning |
|---|---|
| `id` | Stable `BL-` identifier with at least three digits, beginning at `BL-001`. Retain IDs and paths after closure. |
| `title` | Short description of the outcome. |
| `kind` | A `bug` records an observed failure against an evidenced expectation. A `story` delivers one improvement or answers an investigation question. An `epic` groups independently deliverable children. |
| `status` | One of the states below. |
| `priority` | `p1` when delay has a concrete significant cost, `p2` for planned work, `p3` for optional or deferred work. Explain the choice in the item. |
| `target` | Nonempty release or planning horizon. Use "unscheduled" when none is agreed. |
| `applies_to` | Narrow paths relative to the repository root, with forward slashes and trailing slashes for directories. Use "." for repository-wide work. Proposed work may have an empty list. Explain planned new paths. Preserve explicit sibling-repository scopes and resolve them from the checkout root. |
| `tags` | Lowercase kebab-case search terms or an empty list. |
| `parent` | Containing epic ID for a bug or story, otherwise null. Keep the epic's linked child list in agreement. |
| `depends_on` | IDs of prerequisites. Check that each exists and the graph is acyclic. Grouping creates no dependency. An epic must not depend on its own children. |
| `updated` | ISO date of the latest meaningful change. |

## States

| Status | Condition |
|---|---|
| `proposed` | Captured for consideration. Record outstanding questions. |
| `ready` | Evidence is current, scope and acceptance are clear, prerequisites are done, and choices needed to start are settled. An epic has a concrete child breakdown. |
| `in_progress` | Authorized work has started. |
| `blocked` | Record the obstacle and the condition that would unblock the work. |
| `done` | Every acceptance criterion has supporting evidence. Record the observed results, checks actually run, and relevant paths or links. For an epic, verify the overall outcome and account for each required child. |
| `dropped` | Record the reason and any replacement. Reassess dependents and the parent epic's scope. |

For selection, weigh prerequisite order, urgency, and impact. Reassess stale
ready items and dropped prerequisites. Keep the current state concise. Resolve
Markdown links from the file containing them. A scoped path or an item's place
in the queue does not grant permission to change a repository.
