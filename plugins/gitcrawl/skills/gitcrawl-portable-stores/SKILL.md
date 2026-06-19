---
name: gitcrawl-portable-stores
description: Use when Codex needs to configure, inspect, consume, prune, publish, or explain Gitcrawl portable stores, shared Git-backed archive snapshots, portable database behavior, or multi-agent cache distribution.
---

# Gitcrawl Portable Stores

Use portable stores when multiple agents or machines need a shared Git-backed Gitcrawl archive
without each consumer making the same GitHub API calls.

## Configure A Store

```bash
gitcrawl init \
  --portable-store https://github.com/org/gitcrawl-store.git \
  --portable-db data/owner__repo.sync.db \
  --json
```

Add `--store-dir /path/to/checkout` only when the checkout location must be fixed.

## Read Behavior

Read-only commands refresh the portable checkout before reading when possible:

```bash
gitcrawl status --json
gitcrawl search issues "query" -R owner/repo --json number,title,url
gitcrawl clusters owner/repo --json
gitcrawl cluster-detail owner/repo --id 42 --json
gitcrawl tui owner/repo
```

If the remote is unavailable, Gitcrawl answers from the local checkout when it can.

## Write And Publish

Writable operations use a runtime mirror so local vectors and overrides do not dirty the published
portable checkout:

```bash
gitcrawl refresh owner/repo --json
gitcrawl portable prune --body-chars 256 --json
```

After pruning, commit and push the pruned database from the configured portable checkout with the
repository's normal version-control workflow.

## Caveats

- Portable stores carry Gitcrawl SQLite archive snapshots, not Octopool or `gh` command caches.
- Pruned portable v2 data keeps compact thread, comment, PR detail, and fingerprint data.
- Pruning removes large or regenerable data such as raw GitHub JSON, generated documents, FTS
  indexes, embeddings, vectors, code snapshots, cluster run history, and similarity edges.
- Multiple writers can race like any Git-backed workflow; prefer one publisher or CI.

## Rules

- Use `gitcrawl doctor --json` to resolve the active store and runtime paths before diagnosing.
- Do not run `code index` against a portable-store runtime mirror.
- Use `portable prune` before committing a shareable store snapshot.
- Treat publishing policy as a repository/team decision, not a Gitcrawl command default.
