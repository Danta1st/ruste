---
name: my-daily-summary
description: Prepare a personal daily standup from recent GitHub activity and user-provided work. Use for standup updates or summaries of what the user worked on; use the requested period, defaulting to the last 24 hours.
---

# My Daily Summary

Turn verified activity into a concise standup, grouped by repository, with code reviews and blockers.

## 1. Establish scope and collect evidence

Use the requested reporting period and repositories; otherwise cover the last 24 hours across repositories visible to the current GitHub account. Fix the time window once and use it throughout retrieval. Use the user's timezone for the displayed date when known.

For GitHub retrieval, read [GitHub activity](references/gh-commands.md) before running commands. It owns the queries, evidence rules, and retrieval limits. Finish collection when authored PRs, submitted reviews, and authored commits have each been checked, or their retrieval gaps recorded.

If GitHub CLI is unavailable or authentication fails, explain the access problem and ask what the user worked on. Format supplied work under **Other Work**. When only part of retrieval fails, summarize the verified results and state the missing coverage. An access failure means unknown activity, rather than an empty day.

If successful searches yield no verified activity, say: "I couldn't find GitHub activity for this period. What did you work on?" Incorporate their answer; GitHub searches do not cover offline work.

## 2. Build the standup

Group authored work under **owner/repository** headings. Put reviews of other people's PRs under **Code Review**, including the repository on each line. Put additional work supplied by the user under **Other Work**. Omit empty groups.

- Use one line per item, with an explicit action or outcome and a short description of the work. Link PR numbers to their source URLs.
- Use ✅ for merged PRs, 🔄 for opened PRs or authored commits, ⏹️ for PRs closed without merging, and 👁️ for reviews. Keep the action in text so meaning survives without emoji.
- Consolidate several actions on one PR into one line. Group commits in the same repository by theme, counting unique SHAs. Aim for 3–7 lines; use fewer when little activity exists and more when grouping would hide distinct, material work.
- Preserve existing `AB#` references from titles or commit messages inline. Look up Azure DevOps details only when requested and its tools are available.
- Keep user-provided work and blockers faithful to their wording; use verified GitHub actions for activity claims and label current PR status as context.

For a formatting example, read [Example standup](examples/standup-example.md). Its facts are illustrative; use the current user's evidence.

Before presenting, account for every verified item through an individual line or a grouped line. Check actions against their evidence, deduplicate across searches, and include any material coverage limitation. Show the reporting period when it differs from the default.

## 3. Resolve blockers and export

Use blockers already provided. Otherwise, after showing the draft, ask: "Any blockers or things you're waiting on?" A pending reply leaves blockers unconfirmed; write `✅ No blockers` only when the user explicitly confirms none.

Append confirmed blockers below a `---` divider, each prefixed with ⚠️. If blockers remain unconfirmed in a saved draft, label them `Blockers: not yet confirmed`.

Save immediately when an export was already requested. Otherwise offer to save the summary once and wait for the user's choice. Use their path or default to `daily-summary-YYYY-MM-DD.md`; preserve an existing file unless replacement is authorized. Verify that the saved contents match the latest summary and report its path.

Complete when the summary is accounted for against the evidence, blockers are confirmed or explicitly pending, and any requested export has been verified.
