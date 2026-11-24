---
name: my-local-backlog
description: "Maintain a personal Git-checkout backlog. Use when asked to capture a local backlog item, find ready work by scope, resume a BL item, or update or close it with evidence."
---

# My local backlog

Start at the consuming checkout's Git root (`git rev-parse --show-toplevel`).
Use `inbox/backlog/` unless the user identifies another local backlog. Read its
`INDEX.md` first. Follow an existing `PROTOCOL.md` and local templates for that
backlog's conventions. The bundled [protocol](references/protocol.md) supplies
defaults. Resolve bundled resources from this skill's directory. Before
initializing or capturing, read the applicable protocol and templates. If
they are inaccessible, report the missing path before creating local state.

## Find or resume work

For a lookup in a checkout with no backlog, report that it is empty and finish
without writing files. Use the index to shortlist by ID, scope, target, or
priority. Read the matching items and only the parent, prerequisites, or linked
working plan needed for the request. Check current evidence before recommending
ready work. Report the ID, next verifiable outcome, and any concrete blocker.
A request to list or recommend work ends with the answer. Start implementation
only when it is part of the user's request.

## Before writing local state

From the Git root, check `git ls-files -- inbox/backlog/` for tracked or staged
entries. If present, surface the conflict with local-only storage before writing.
Use `git check-ignore -v -- inbox/backlog/INDEX.md` to inspect the effective
rule. If protection is missing, append the anchored directory rule
`/inbox/backlog/` on its own line in the file given by
`git rev-parse --git-path info/exclude`. Preserve existing entries. Verify
`git check-ignore -q -- inbox/backlog/INDEX.md` and each intended backlog file
before writing. Substitute the actual repository-relative path for a custom
location. If Git cannot protect the files, report the obstacle.

On first capture or explicit initialization, create only missing directories
and seed a missing index from [the empty index](assets/index.md). For an
existing collection with items but no index, recover the rows from item
metadata. Keep IDs, filenames, narrative decisions, local templates, and links
to working plans when adopting a backlog.

## Capture or update an item

1. Check both the index and item metadata for an existing outcome. For new work
   choose the local template or the bundled [bug](assets/bug.md),
   [story](assets/story.md), or [epic](assets/epic.md). Allocate the next ID
   above the highest recorded ID, including closed items. Store it in
   `items/bl-NNN-short-slug.md`. Fill in evidence, uncertainty, scope, and
   observable acceptance at the level known. When the user supplies acceptance
   criteria, copy their wording into the checklist, one criterion per checkbox.
   Draft criteria only when none were supplied. Put suggested checks and
   reporting details in the evidence section.
2. Apply the requested change using the [status and relationship rules](references/protocol.md).
   On closure, compare each criterion with actual results. If evidence is
   missing, identify it and keep the item open. When dropping work, record why
   and any successor. Keep closed items addressable.
3. Update the item's date on meaningful changes and edit its index row to match
   the frontmatter and title. Preserve surrounding prose and unrelated rows.
   Recheck IDs against the directory and index before finishing, especially if
   another session edited the backlog. Check changed links, relationships, and
   effective Git exclusion. Return the ID and outcome.
