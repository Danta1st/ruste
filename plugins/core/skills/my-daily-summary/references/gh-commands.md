# GitHub activity

Use this reference for GitHub retrieval. Commands below use Bash syntax. Check the installed `gh --version` and relevant `--help` if a flag or field is rejected; adapt to that version using the [official CLI manual](https://cli.github.com/manual/).

## Identity and reporting window

Confirm CLI availability, then resolve the active authenticated account:

```bash
gh --version
standup_login=$(gh api user --jq '.login')
```

Proceed only when the identity lookup succeeds. Use the configured GitHub host and honor any repository scope supplied by the user; add `--repo owner/repository` to searches when scoped to one repository.

Set `standup_since` and `standup_until` to the selected UTC interval. For the default 24-hour window, this computes both boundaries from one instant when Python 3 is available:

```bash
read -r standup_since standup_until < <(python3 - <<'PY'
from datetime import datetime, timedelta, timezone
end = datetime.now(timezone.utc).replace(microsecond=0)
print(*(t.strftime('%Y-%m-%dT%H:%M:%SZ') for t in (end - timedelta(hours=24), end)))
PY
)
```

For a requested interval, or if Python 3 is unavailable, set the two variables from the requested dates or another available clock. Compare parsed timestamps using `standup_since <= timestamp <= standup_until`.

## Find candidates

Run all three searches. A PR's update timestamp identifies a candidate; later verification establishes the action.

```bash
gh search prs --author "$standup_login" --updated ">=$standup_since" \
  --sort updated --order desc --limit 100 \
  --json number,title,url,repository,createdAt

gh search prs --reviewed-by "$standup_login" --updated ">=$standup_since" \
  --sort updated --order desc --limit 100 \
  --json number,title,url,repository,author

gh search commits --author "$standup_login" \
  --author-date "$standup_since..$standup_until" --merge=false \
  --sort author-date --order desc --limit 100 \
  --json sha,repository,commit,author,committer,parents,url
```

When a search returns its full limit, increase the limit until the result count falls below it. If a search ceiling, timeout, or rate limit prevents this, narrow by repository or time interval and deduplicate overlapping results. If coverage remains incomplete, report the summary as partial. Search results cover indexed, accessible activity; avoid claiming coverage of every repository or all work.

## Verify PR actions and reviews

Deduplicate PR candidates by full repository name and PR number. For authored PR candidates, use the returned URL:

```bash
gh pr view "$standup_pr_url" \
  --json number,title,url,author,createdAt,mergedAt,mergedBy,closedAt,state
```

For each review candidate, substitute its repository and PR number, and fetch all review pages:

```bash
gh api "repos/OWNER/REPO/pulls/NUMBER/reviews?per_page=100" \
  --paginate --jq '.[] | {id, user: .user.login, submitted_at, state, html_url}'
```

| Summary claim | Required evidence in the reporting window |
|---|---|
| Opened PR | `author.login` matches the current user and `createdAt` is in range |
| Their PR merged | `mergedAt` is in range; use "PR merged" unless `mergedBy.login` also identifies the user |
| Their PR closed without merging | `closedAt` is in range and `mergedAt` is absent; describe closure without implying delivery or who closed it |
| Reviewed PR | Review `user` matches the current login and `submitted_at` is present and in range; pending reviews are unfinished |
| Approved / requested changes | The matching submitted review's `state` supports that specific outcome |

A historical review plus a recent update by someone else is insufficient evidence of a review in this period. Combine multiple qualifying reviews of one PR into one line; retain any meaningful change in outcome. Exclude reviews of the user's own PRs from **Code Review**.

## Verify commits

Keep commits whose `author.login` matches the account and whose `commit.author.date` is in range. Identify repositories by `repository.fullName` in commit-search output and `repository.nameWithOwner` in PR-search output; use the URL if those fields differ in the installed version.

Deduplicate by repository and SHA. Exclude merge commits by parent count and bot-authored commits by account login when available. A bot committer can publish a human-authored change, so committer identity alone is insufficient to discard it.

Describe these as authored commits. Their timestamps establish authorship dates; report a push only when separate push-event evidence is available. If commits appear to repeat a PR item, combine them only when the relationship is verified; title similarity alone is insufficient.

Collection is complete when each candidate has qualifying evidence or a reason for exclusion, and every failed or truncated retrieval is accounted for.

## Official references

- [PR search](https://cli.github.com/manual/gh_search_prs) and [PR details](https://cli.github.com/manual/gh_pr_view)
- [Commit search](https://cli.github.com/manual/gh_search_commits)
- [Submitted reviews and their fields](https://docs.github.com/en/rest/pulls/reviews#list-reviews-for-a-pull-request)
