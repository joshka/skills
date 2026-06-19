---
name: gitcrawl-governance
description: Use when Codex needs to apply or inspect Gitcrawl local maintainer governance decisions, including close-thread, close-cluster, reopen actions, cluster member exclusion or inclusion, and canonical cluster member selection.
---

# Gitcrawl Governance

Use governance commands to record local maintainer decisions on top of Gitcrawl clusters.
These commands do not edit GitHub issues, labels, comments, or PRs.

## Local Close And Reopen

```bash
gitcrawl close-thread owner/repo --number 123 --reason "duplicate handled" --json
gitcrawl close-thread owner/repo \
  --number https://github.com/owner/repo/issues/123 \
  --reason "handled" \
  --json
gitcrawl reopen-thread owner/repo --number 123 --json

gitcrawl close-cluster owner/repo --id 42 --reason "consolidated under #123" --json
gitcrawl reopen-cluster owner/repo --id 42 --json
```

Use local close after upstream work is handled or when a thread should disappear from the local
triage view without changing GitHub state.

## Member Overrides

```bash
gitcrawl exclude-cluster-member owner/repo --id 42 --number 456 --reason "different repro" --json
gitcrawl include-cluster-member owner/repo --id 42 --number 456 --json
gitcrawl set-cluster-canonical owner/repo --id 42 --number 123 --reason "main tracking issue" --json
```

Use exclusion for false positives. Use canonical selection when the cluster representative should
be a specific issue or PR.

## Inspect Decisions

```bash
gitcrawl cluster-detail owner/repo --id 42 --body-chars 600 --json
gitcrawl clusters owner/repo --hide-closed --json
gitcrawl durable-clusters owner/repo --include-closed --json
```

Prefer CLI JSON for stable inspection. Use read-only SQLite only when the user asks for exact
override history not exposed by CLI output.

## Rules

- Always include a concise reason when recording an override.
- Verify the target cluster or member with `cluster-detail` before changing local governance.
- Use inverse commands rather than direct database edits.
- State clearly that governance is local-only and not a GitHub write-back path.
- Use live GitHub tooling separately when comments, labels, or issue closure are required.
