---
name: discrawl-search
description: Use when Codex needs to search or inspect a local Discrawl Discord archive, including FTS, semantic or hybrid search, message slices, channel, author, guild, date, DM scoping, JSON output, and adjacent context gathering.
---

# Discrawl Search

Use Discrawl local reads before Discord search when the task is archive discovery,
conversation reconstruction, duplicate context, or historical message lookup.

## Workflow

1. Resolve scope: guild, channel, DM, author, query terms, dates, and result limit.
2. Check freshness first for recent or current claims with `discrawl status --json`.
3. Prefer JSON output for agent workflows and exact follow-up filtering.
4. Use `messages` for chronological slices and `search` for keyword or vector discovery.
5. Report absolute date spans, result counts, channel or DM names, and known gaps.

## Search

```bash
DISCRAWL_NO_AUTO_UPDATE=1 discrawl --json search "panic: nil pointer"
discrawl search --mode fts "panic: nil pointer"
discrawl search --mode semantic "missing launch checklist"
discrawl search --mode hybrid "database timeout"
discrawl search --guild 123456789012345678 "payment failed"
discrawl search --channel '#maintainers' --author Josh --limit 50 "release"
discrawl search --dm "launch checklist"
```

Use `fts` for exact local text search. Use `hybrid` when embeddings may help but FTS fallback is
acceptable. Use `semantic` only when embeddings are configured and populated.

## Message Slices

```bash
discrawl messages --channel '#maintainers' --days 7 --all
discrawl messages --channel general --hours 24 --json
discrawl messages --guild 123456789012345678 --channel 111 --last 50
discrawl messages --dm --last 50
```

Use message slices after search to recover adjacent conversation, exact chronology, and reply
context.

## Rules

- Use `DISCRAWL_NO_AUTO_UPDATE=1` when a read smoke should not import Git snapshots.
- Do not claim live Discord state from local archive data without checking freshness.
- Prefer local archive evidence first; hit Discord APIs only when the archive is stale, missing
  scope, or the user asks for current external context.
- Treat `--include-empty` as an explicit choice for attachment, embed, reply, or sparse rows.
