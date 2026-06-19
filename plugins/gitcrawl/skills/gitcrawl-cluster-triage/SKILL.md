---
name: gitcrawl-cluster-triage
description: Use when Codex needs to inspect, explain, tune, or triage Gitcrawl duplicate clusters, cluster reports, similar neighbors, durable clusters, or the Gitcrawl TUI for maintainer issue and pull request review.
---

# Gitcrawl Cluster Triage

Use clusters to group related issues and pull requests for maintainer triage. Cluster commands
read local archive data and local vectors; refresh or embed first when vectors are missing.

## List And Report

```bash
gitcrawl clusters owner/repo --sort size --min-size 5 --json
gitcrawl clusters owner/repo --sort recent --limit 20 --json
gitcrawl clusters-report owner/repo --limit 10 --min-size 5
gitcrawl clusters-report owner/repo --limit 10 --min-size 5 --json
gitcrawl durable-clusters owner/repo --include-closed --json
```

Use `clusters` for the normal display view. Use `durable-clusters --include-closed` for an
audit-style view of durable rows and historical local decisions.

## Inspect

```bash
gitcrawl cluster-detail owner/repo --id 42 --body-chars 600 --json
gitcrawl cluster-explain owner/repo --id 42 --body-chars 600 --json
gitcrawl neighbors owner/repo --number 123 --limit 10 --json
```

Use `neighbors` for "what else looks like this?" before deciding whether a new report is a
duplicate.

## Generate Or Tune

```bash
gitcrawl cluster owner/repo --json
gitcrawl cluster owner/repo --threshold 0.85 --cross-kind-threshold 0.95 --json
gitcrawl cluster owner/repo --threshold 0.75 --k 24 --json
gitcrawl cluster owner/repo --max-cluster-size 20 --json
```

Tighten thresholds when unrelated reports merge. Lower threshold or increase `--k` when obvious
duplicates split apart. Inspect with `cluster-detail` before accepting tuning changes.

## TUI

```bash
gitcrawl tui owner/repo
gitcrawl tui owner/repo --min-size 5 --sort size
gitcrawl tui owner/repo --layout focus
```

Use the TUI for interactive cluster review, member detail, jump-to-number, neighbors, and local
governance actions.

## Rules

- Run `gitcrawl refresh owner/repo` when sync, embeddings, and clusters all need to be current.
- Use `--hide-closed` to focus on currently active local triage.
- Treat cluster output as local triage evidence; verify live GitHub before upstream writes.
- Do not use `gitcrawl gh`; that surface moved to Octopool.
