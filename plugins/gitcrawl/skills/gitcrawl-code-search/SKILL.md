---
name: gitcrawl-code-search
description: Use when Codex needs to index a local Git checkout into Gitcrawl or search source files alongside GitHub issue and pull request archive data with --scope code or --scope all.
---

# Gitcrawl Code Search

Use code indexing when a local checkout should be searchable through the same Gitcrawl archive
workflow as GitHub threads. Code documents are separate from issue and PR documents.

## Index A Checkout

Mirror the repository first, then index tracked UTF-8 text files:

```bash
gitcrawl sync owner/repo --json
gitcrawl code index owner/repo --path /path/to/checkout --json
```

Optional bounds:

```bash
gitcrawl code index owner/repo \
  --path /path/to/checkout \
  --max-file-bytes 524288 \
  --max-total-bytes 268435456 \
  --max-files 100000 \
  --json
```

## Search Source

```bash
gitcrawl search owner/repo --query "RefreshManifest" --scope code --json
gitcrawl search owner/repo --query "manifest cache" --scope all --mode hybrid --json
```

Use `--scope code` for source-only search. Use `--scope all` when both archive threads and source
matches are useful.

## Boundaries

- Only tracked regular text files are indexed.
- Untracked files, symlinks, submodules, invalid UTF-8 files, and oversized files are skipped.
- Each successful index replaces the prior code snapshot for that repository.
- Code documents are not embedded and do not participate in neighbors or duplicate clusters.
- Portable-store runtime mirrors reject `code index`; index from a normal local database.

## Rules

- Pass the exact checkout path with `--path`; do not assume the current directory is right.
- Use `git status` or `jj status` separately when dirty worktree state matters to the answer.
- Treat source search as local discovery; inspect the actual files before making code claims.
