# Repository onboarding

Read this reference only when the user asks to analyze, onboard, adopt, or
verify repository knowledge. The mode names are natural-language invocation
conventions, not CLI subcommands or a permission mechanism.

If the user asks to onboard a repository without naming a phase, use
`onboard analyze`. Never infer permission to adopt from permission to analyze.

An onboarding lifecycle starts with an analysis plan for one active repository
root and ends when adoption is verified or the plan is abandoned. Its approved
candidate table is the registry for small positive integer candidate IDs.
Candidate IDs are workflow references, not OKF concept IDs: never write them
into concept frontmatter, indexes, concept IDs, or `Knowledge:` handoffs.

## `onboard analyze`

Analyze the repository and produce an adoption plan. Make no file changes.

1. Establish the repository root and requested scope. For a large repository,
   analyze one bounded context, service, or area at a time.
2. Read repository instructions, existing indexes, ADRs, architecture and
   domain documentation, proposals, runbooks, and other task-relevant knowledge
   sources. Do not crawl every source file or generated document.
3. Detect relevant library, interface, data, and operational capabilities.
   Inspect only representative code, tests, schemas, migrations, manifests, and
   configuration needed to verify candidate claims.
4. Classify each relevant source or claim as one of:
   - approved durable knowledge;
   - unresolved proposal;
   - executable source to link rather than copy;
   - volatile status, task history, or incident timeline to exclude;
   - duplicate knowledge; or
   - conflict requiring resolution.
5. Apply the admission gate provisionally. Accepted prose is evidence, not
   active OKF knowledge until adoption is authorized and completed.
6. Recommend whether to preserve an existing documentation root or create
   `docs/knowledge/`. Account for path-derived concept IDs before recommending
   any move.
7. Choose the initial row order deliberately, using recommendation priority or
   report structure. Then assign consecutive IDs starting at `1` in that order.
   Give every candidate-table row an ID, including an ineligible row. General
   background and exclusions listed outside the table do not require IDs.

Produce an adoption report containing:

- analysis scope and detected capabilities;
- recommended bundle root and rationale;
- candidate table with explicit `ID`, source or claim, proposed type, area,
  authority, evidence or canonical resource, admission result, and recommended
  action columns;
- proposals and volatile material that must remain outside active knowledge;
- duplicates, broken evidence, and conflicts;
- the first one to three concepts recommended for a pilot;
- the exact root, candidate IDs, and mutation actions that require user
  approval; and
- a paste-ready approval block that names the proposed root, eligible and
  ineligible IDs, selected IDs, and required addition, conversion, move,
  replacement, index, or link actions.

The table must make identity, eligibility, and authorization-relevant action
visible. For example:

| ID | Source or claim | Proposed type | Admission | Proposed action |
|---:|---|---|---|---|
| 1 | `docs/decisions/audit-dependency.md` | Decision | Eligible | Convert in place |
| 2 | `docs/proposals/cache-store.md` | — | Not eligible: unresolved | Leave outside active knowledge |

```text
Proposed bundle root: docs/decisions/
Eligible candidate IDs: 1
Ineligible candidate IDs: 2

Approval required:
- Adopt ID 1.
- Convert its source in place.
- Add or update only the affected OKF indexes and links.
```

### Refresh an analysis

When refreshing the same lifecycle, require the prior table and the same active
repository root. Resolve existing IDs through both the prior source and claim
details; current row order never defines identity.

- Preserve the ID when the logical candidate is unchanged, including after an
  editorial source change or an unambiguous move of the same claim.
- Assign a new candidate the next unused integer, regardless of where its row
  appears. Never renumber existing rows or compact gaps.
- Keep a disappeared candidate's ID visible as unavailable when an outstanding
  approval could refer to it. Retire IDs whose claims changed meaning, split,
  merged, or cannot be matched unambiguously. Never reuse unavailable or
  retired IDs during the lifecycle.
- Recheck evidence after editorial changes. Require approval again when a move
  changes an action or destination.
- When source and claim details do not resolve one prior row, stop for
  clarification. Retire the old ID and assign clarified replacement candidates
  the next unused IDs.

If the prior table is unavailable or the repository root differs, do not
reconstruct or resolve its integers. Begin a new lifecycle at `ID 1` and state
that IDs from the previous lifecycle no longer apply.

Do not create a bundle, convert documents, or edit repository instructions.
Finish with `Knowledge: none` unless active existing knowledge was applied or a
conflict requires the standard conflict handoff.

## `onboard adopt`

Apply only an explicitly approved onboarding plan.

Before writing, require all of:

- the approved analysis table for the current lifecycle;
- the same active repository root used by that analysis;
- an approved bundle root;
- an explicit list of positive integer candidate IDs to adopt;
- approval for any concept addition or document conversion, move, or
  replacement; and
- identifiable authority and resolvable evidence for each candidate.

If any item is missing, stop and return to `onboard analyze`; do not choose on
the user's behalf.

Resolve every requested ID through the table to exactly one row. Confirm that
each row remains eligible, then re-read its source, authority, and executable
evidence for drift or conflict. When exactly one approved plan is active in
context, `adopt 1 and 3` is valid shorthand for `Adopt IDs 1 and 3`; the
integers still refer to the `ID` column, never row positions.

Malformed, unknown, unavailable, retired, ambiguous, or ineligible IDs stop all
writes. Material drift also stops adoption until a refreshed analysis is
approved. Do not guess from a title or current ordering. Selecting an ID
approves only its candidate row; it does not approve the bundle root, source
addition, conversion, movement, replacement, deletion, application-code
changes, or any other candidate.

For an authorized adoption:

1. Recheck the approved sources and relevant executable artifacts for drift or
   conflict.
2. Preserve the approved root. Do not create `docs/knowledge/` when another
   root was selected.
3. Convert or add only the selected candidates. Preserve the operative meaning
   and useful history of an existing approved document.
4. Keep unresolved proposals, task history, incident timelines, and volatile
   status outside active indexes.
5. Link authoritative schemas, migrations, manifests, configuration, or code;
   do not reproduce their machine-readable contents.
6. Add or update the concept and every affected index or link atomically. Do not
   change application code, evidence, or unrelated documentation.
7. Validate frontmatter, descriptions, paths, lifecycle, resources, and the
   preservation of unknown producer fields.

Use `Knowledge: updated` for an in-place conversion and `Knowledge: added` only
for a newly created concept path. Use the other standard handoffs when
applicable. A completed analysis alone never authorizes these mutations.

## `onboard verify`

Evaluate an adopted bundle without modifying it.

1. Validate the root and area indexes, concept frontmatter, non-empty `type`,
   exact index descriptions, lifecycle, local resources, and relevant links.
2. Exercise representative retrieval probes:
   - a task that should retrieve active knowledge;
   - an ordinary local task that should retrieve none; and
   - an unresolved proposal that must remain non-authoritative.
3. Exercise an existing conflict scenario when one is already present. Do not
   manufacture a conflict by editing code, schemas, or knowledge.
4. For each probe, report the concepts selected, concepts excluded, authority
   outcome, and expected `Knowledge:` result.
5. Report recall, precision, authority, conflict, and structural failures. Do
   not repair them unless the user separately authorizes fixes.

When the approved plan for the same lifecycle is available, also report each
relevant ID with its adoption and verification outcome:

| ID | Adoption result | Verification result |
|---:|---|---|
| 1 | `docs/decisions/audit-dependency.md` | Active and retrievable |
| 2 | Not adopted | Remains outside active knowledge |

If the plan is unavailable, verify by repository path and do not reconstruct
lifecycle IDs. Plan availability affects ID traceability, not whether path-based
bundle verification can proceed.

Finish with one standard handoff for the verification task, listing active
concepts that materially informed it. Probe results belong in the report and do
not replace the single final handoff. The handoff always uses
repository-relative paths, never candidate IDs.
