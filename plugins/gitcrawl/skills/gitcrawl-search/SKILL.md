---
name: gitcrawl-search
description: Use when Codex needs local cached GitHub issue or pull request discovery with Gitcrawl, including duplicate-first searches, gh search-compatible query shapes, bounded staleness with --sync-if-stale, or deciding when to verify with live GitHub after local discovery.
---

# Gitcrawl Search

Use Gitcrawl local search before live GitHub search when the task is discovery, duplicate
triage, or finding related issues and pull requests in a repository mirror.

## Workflow

1. Identify the target `owner/repo`, query terms, item kind, state, and output fields.
2. For recent or current results, bound staleness with `--sync-if-stale`.
3. Prefer JSON output for agent workflows.
4. Treat Gitcrawl search as discovery. Verify with `gh` or another live GitHub path before
   commenting, labeling, closing, merging, or making a current-state claim.

## Commands

Direct repository search:

```bash
gitcrawl search owner/repo --query "download stalls" --mode hybrid --limit 30 --json
gitcrawl search owner/repo --query "RefreshManifest" --scope code --json
gitcrawl search owner/repo --query "manifest cache" --scope all --json
```

`gh search`-compatible issue and PR discovery:

```bash
gitcrawl search issues "manifest cache" \
  -R owner/repo \
  --state open \
  --sync-if-stale 5m \
  --json number,title,state,url,updatedAt,labels \
  --limit 30

gitcrawl search prs "manifest cache" \
  -R owner/repo \
  --state open \
  --json number,title,state,url,updatedAt,isDraft,author \
  --limit 20
```

Hydrate a candidate batch:

```bash
nums="$(gitcrawl search issues "checksum mismatch" -R owner/repo \
  --json number --limit 30 | jq -r '[.[].number] | join(",")')"

gitcrawl sync owner/repo --numbers "$nums" --include-comments --with pr-details
```

## Rules

- Use `--mode hybrid` for robust direct search unless the task needs exact keyword-only behavior.
- Use `--scope code` or `--scope all` only after `gitcrawl code index` has populated source docs.
- Use `--state open` for active triage, `--state all` only when historical results matter.
- Do not use `gitcrawl gh`; that compatibility cache moved to Octopool.
