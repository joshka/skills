---
name: discrawl-sync-freshness
description: Use when Codex needs to initialize, diagnose, refresh, repair, or inspect freshness for a Discrawl Discord archive, including status --json, doctor, bot sync, wiretap sync, targeted guild or channel refreshes, and sync-source choices.
---

# Discrawl Sync Freshness

Use freshness checks before recent/current analysis and before deciding whether local archive data
is complete enough for the user's question.

## Health

```bash
discrawl status --json
discrawl doctor
discrawl doctor --json
discrawl metadata --json
```

Use `status --json` for archive counts and freshness. Use `doctor` for config, token, database,
FTS, guild, and local setup diagnostics.

For precise default database freshness:

```bash
case "$(uname -s)" in
  Darwin) db="$HOME/Library/Application Support/discrawl/discrawl.db" ;;
  *) db="${XDG_DATA_HOME:-$HOME/.local/share}/discrawl/discrawl.db" ;;
esac

sqlite3 "$db" \
  "select coalesce(max(updated_at),'') from sync_state where scope like 'channel:%';"
```

## Sync Choices

```bash
discrawl sync
discrawl sync --source discord
discrawl sync --source wiretap
discrawl sync --update=auto
discrawl sync --guild 123456789012345678
discrawl sync --channels 111,222 --since 2026-03-01T00:00:00Z
discrawl sync --all-channels
discrawl sync --full
```

Use plain `sync` for routine refresh. Use `--source discord` to repair bot-visible guild data.
Use `--source wiretap` for local Discord Desktop cache import only. Use `--full` only for
deliberate historical backfills.

## Rules

- Do not invent bot-token availability. Token sources are config, env, or OS keyring.
- If SQLite reports busy or locked, check for stray `discrawl` processes before retrying.
- Use targeted guild, channel, and `--since` flags before broad backfills when possible.
- Use `--all-channels` for broad incremental repair without marking older history complete.
- When the installed binary lacks a feature, verify against a current `openclaw/discrawl`
  checkout before concluding the feature is missing.
