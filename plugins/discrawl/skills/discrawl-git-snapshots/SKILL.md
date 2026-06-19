---
name: discrawl-git-snapshots
description: Use when Codex needs to subscribe to, update, publish, filter, inspect, or explain Discrawl private Git-backed archive snapshots, including public-only filters, media and embedding export caveats, and token-free reader setup.
---

# Discrawl Git Snapshots

Use Git-backed snapshots to share non-DM Discord archive data through a private repository without
requiring every reader to have Discord bot credentials.

## Subscriber

```bash
discrawl subscribe https://github.com/example/discord-archive.git
discrawl subscribe --stale-after 15m https://github.com/example/discord-archive.git
discrawl subscribe --no-auto-update https://github.com/example/discord-archive.git
discrawl update
discrawl search "launch checklist"
discrawl messages --channel general --hours 24
```

Subscriber setup writes token-free config, imports the snapshot, and disables live `sync` and
`tail` because they require Discord credentials.

## Publisher

```bash
discrawl publish --remote https://github.com/example/discord-archive.git --push
discrawl publish --readme path/to/discord-backup/README.md --push
discrawl publish --message "sync: discord archive" --push
discrawl publish --with-embeddings --push
discrawl publish --no-media --push
```

Use an already-synced local archive for publishing. Publishing exports cached media only; it does
not download missing Discord CDN media.

## Filters

```bash
discrawl publish --public-only --no-media --push
discrawl publish --public-only --include-channels 1458141495701012561 --push
discrawl publish --exclude-channels 111,222 --push
```

Filters narrow only the published snapshot. The local SQLite archive can remain richer than the
reader-facing Git snapshot.

## Rules

- Never publish `@me` DM rows, local DM media, wiretap sync state, or secrets.
- Do not use `--readme` with active publish filters; filtered reports can leak full-archive totals.
- Explain that `--public-only` is based on `@everyone` channel visibility and permission
  overwrites.
- Use `--with-embeddings` only when compatible local vectors exist and should be shared.
- Treat remote and branch policy as a repository/team decision.
