---
name: gitcrawl-sync-refresh
description: Use when Codex needs to initialize, diagnose, refresh, hydrate, embed, or inspect run history for a Gitcrawl local GitHub issue and pull request archive, including doctor/status checks, targeted --numbers syncs, full backfills, and refresh freshness decisions.
---

# Gitcrawl Sync And Refresh

Use this skill when the archive needs setup, health checks, freshness checks, targeted
hydration, embeddings, clustering, or run history inspection.

## Health And Inventory

```bash
gitcrawl doctor --json
gitcrawl status --json
gitcrawl metadata --json
```

Use `doctor` to resolve active config, database paths, token visibility, model settings,
portable-store state, repository counts, thread counts, cluster counts, and last sync time.

## Sync Choices

Default incremental sync:

```bash
gitcrawl sync owner/repo --json
```

First historical backfill:

```bash
gitcrawl sync owner/repo --state all --include-comments --json
```

Exact issue or PR hydration:

```bash
gitcrawl sync owner/repo --numbers 123,456 --include-comments --with pr-details --json
gitcrawl sync owner/repo --numbers https://github.com/owner/repo/issues/123 --with pr-details --json
```

`--numbers` accepts bare numbers, `#123`, `issues/123`, `pull/123`,
`owner/repo#123`, and full GitHub issue or pull request URLs.

## Refresh And Embed

Run the full sync -> embed -> cluster path:

```bash
gitcrawl refresh owner/repo --json
```

Run only selected stages:

```bash
gitcrawl refresh owner/repo --no-sync --json
gitcrawl refresh owner/repo --no-embed --no-cluster --json
gitcrawl embed owner/repo --number 123 --json
```

Inspect run history:

```bash
gitcrawl runs owner/repo --kind sync --limit 10 --json
gitcrawl runs owner/repo --kind embedding --limit 10 --json
gitcrawl runs owner/repo --kind cluster --limit 10 --json
```

## Rules

- Use targeted `--numbers` sync before deep analysis of a specific issue or PR.
- Add `--include-comments` when conversation context matters.
- Add `--with pr-details` when PR files, commits, checks, or workflow runs matter.
- Use `--state all` for deliberate backfills, not routine refreshes.
- Use `OPENAI_API_KEY` or configured credentials for embedding and refresh workflows that embed.
- Do not use `gitcrawl gh`; use Gitcrawl for the local mirror and Octopool or live `gh` for
  pooled or current GitHub reads.
