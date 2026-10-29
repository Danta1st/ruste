---
name: create-pr
description: ALWAYS use this skill when creating pull requests — never create a PR directly without it. Follows conventions for PR titles, descriptions, and issue references. Trigger on any create PR, open PR, submit PR, make PR, push and create PR, or prepare changes for review task.
---

# Create Pull Request

Create pull requests following Sentry's engineering practices.

**Requires**: GitHub CLI (`gh`) authenticated and available.

## Prerequisites

Before creating a PR, ensure all changes are committed. If there are uncommitted changes, run the `core:commit` skill first to commit them properly.

```bash
# Check for uncommitted changes
git status --porcelain
```

If the output shows any uncommitted changes (modified, added, or untracked files that should be included), invoke the `core:commit` skill before proceeding.

## Process

### Step 1: Verify Branch State

```bash
# Detect the default branch
BASE=$(gh repo view --json defaultBranchRef --jq '.defaultBranchRef.name')

# Check current branch and status
git status
git log $BASE..HEAD --oneline
```

Ensure:
- All changes are committed
- Branch is up to date with remote
- Changes are rebased on the base branch if needed

### Step 2: Analyze Changes

Review what will be included in the PR:

```bash
# See all commits that will be in the PR
git log $BASE..HEAD

# See the full diff
git diff $BASE...HEAD
```

Understand the scope and purpose of all changes before writing the description.

### Step 3: Write the PR Description

Use this structure for PR descriptions (ignoring any repository PR templates):

```markdown
AB#<issue number>

## Why
<brief description of why these changes are being made>

## What
<brief description of what the PR does>

## Review Order
<recommended order to review the PR>
```

**Do NOT include:**
- "Test plan" sections
- Checkbox lists of testing steps
- Redundant summaries of the diff

**Do include:**
- Clear explanation of why and what
- Links to relevant issues or tickets
- Context that isn't obvious from the code
- Notes on specific areas that need careful review

### Step 4: Create the PR

```bash
gh pr create --draft --title "<type>(<scope>): <description>" --body "$(cat <<'EOF'
<description body here>
EOF
)"
```

**Title format** follows commit conventions:
- `feat(scope): Add new feature`
- `fix(scope): Fix the bug`
- `refactor: Refactor something`

## PR Description Examples

### Feature PR

```markdown
AB#1234

## Why
Alert update and resolution notifications were creating separate Slack messages,
which fragmented context and increased channel noise.

## What
Post update and resolution notifications as replies in the original alert thread
instead of sending a new top-level message.

## Review Order
1. `plugins/slack/notifications.ts` thread selection + reply posting logic
2. `plugins/slack/formatters.ts` message payload changes for threaded replies
3. Tests covering updated/resolved alert flows
```

### Bug Fix PR

```markdown
AB#5678

## Why
Soft-deleted accounts could return `null` from the user API, and downstream
code assumed a user object, causing dashboard crashes.

## What
Add a null guard in the user endpoint and return a 404 response when the user
record is missing.

## Review Order
1. `api/users/get-user.ts` null handling + 404 response mapping
2. `api/users/schema.ts` response contract update (if applicable)
3. Regression tests for soft-deleted user requests
```

### Refactor PR

```markdown
AB#9999

## Why
Validation logic was duplicated across alerts, issues, and projects endpoints,
which made rule updates error-prone and inconsistent.

## What
Move shared validation logic into a common validator module and update each
endpoint to use it. This is a refactor with no intended behavior change.

## Review Order
1. `lib/validation/shared-validator.ts` new shared validator implementation
2. Endpoint updates in `api/alerts/*`, `api/issues/*`, and `api/projects/*`
3. Existing validation tests to confirm behavior parity
```

## Issue References

Reference issues in the PR body:

| Syntax          | Effect                                   |
|-----------------|------------------------------------------|
| `Fixes #1234`   | Closes GitHub issue on merge             |
| `Fixes AB#1234` | Closes Azure Boards issue on merge       |
| `Refs #1234`    | Links Github issue without closing       |
| `Refs AB#123`   | Links Azure Boards issue without closing |

## Guidelines

- **One PR per feature/fix** - Don't bundle unrelated changes
- **Keep PRs reviewable** - Smaller PRs get faster, better reviews
- **Explain the why** - Code shows what; description explains why
- **Mark WIP early** - Use draft PRs for early feedback

## Editing Existing PRs

If you need to update a PR after creation, use `gh api` instead of `gh pr edit`:

```bash
# Update PR description
gh api -X PATCH repos/{owner}/{repo}/pulls/PR_NUMBER -f body="$(cat <<'EOF'
Updated description here
EOF
)"

# Update PR title
gh api -X PATCH repos/{owner}/{repo}/pulls/PR_NUMBER -f title='new: Title here'

# Update both
gh api -X PATCH repos/{owner}/{repo}/pulls/PR_NUMBER \
  -f title='new: Title' \
  -f body='New description'
```

Note: `gh pr edit` is currently broken due to GitHub's Projects (classic) deprecation.
