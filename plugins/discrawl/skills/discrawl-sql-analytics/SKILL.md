---
name: discrawl-sql-analytics
description: Use when Codex needs read-only SQL, counts, rankings, schema-aware diagnostics, exact archive statistics, or controlled admin SQL boundaries for a local Discrawl SQLite database.
---

# Discrawl SQL Analytics

Use `discrawl sql` for exact counts, joins, diagnostics, and rankings when normal CLI reads are too
coarse. The default SQL connection is read-only.

## Commands

```bash
DISCRAWL_NO_AUTO_UPDATE=1 discrawl --json sql \
  "select count(*) as messages from messages;"

DISCRAWL_NO_AUTO_UPDATE=1 discrawl --json sql \
  "select coalesce(nullif(c.name, ''), m.channel_id) as channel,
    count(*) as messages
    from messages m
    left join channels c on c.id = m.channel_id
    group by m.channel_id
    order by messages desc
    limit 20;"

DISCRAWL_NO_AUTO_UPDATE=1 discrawl --json sql \
  "select coalesce(
      nullif(mm.display_name, ''),
      nullif(mm.global_name, ''),
      nullif(mm.username, ''),
      m.author_id
    ) as author,
    count(*) as messages
    from messages m
    left join members mm on mm.guild_id = m.guild_id and mm.user_id = m.author_id
    group by m.guild_id, m.author_id
    order by messages desc
    limit 20;"
```

Read SQL from stdin when quoting becomes awkward:

```bash
printf '%s\n' 'select guild_id, count(*) from messages group by guild_id;' |
  DISCRAWL_NO_AUTO_UPDATE=1 discrawl --json sql -
```

## Schema Notes

- Threads are stored as channels because that matches the Discord model.
- Proven local DMs use the synthetic guild id `@me`.
- Startup fails fast when a local database schema is newer than the supported binary.
- `PRAGMA user_version` identifies the migrated schema version.

## Rules

- Prefer `discrawl --json sql` for agent parsing.
- Keep queries read-only unless the user explicitly requests an admin mutation.
- Never use `--unsafe --confirm` without reviewing the write and stating the mutation target.
- Use `DISCRAWL_NO_AUTO_UPDATE=1` for exact local database reads that must not import snapshots.
