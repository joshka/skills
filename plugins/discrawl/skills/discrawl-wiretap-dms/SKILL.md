---
name: discrawl-wiretap-dms
description: Use when Codex needs to import or inspect local Discord Desktop cache data with Discrawl wiretap, search or list local DMs under @me, run wiretap watch loops, or enforce local-only privacy boundaries for desktop-cache rows.
---

# Discrawl Wiretap DMs

Use wiretap for local Discord Desktop cache import and searchable local DMs. This is not a live DM
sync path and does not use Discord as the user.

## Import

```bash
discrawl wiretap
discrawl wiretap --path "$HOME/Library/Application Support/discord"
discrawl wiretap --dry-run
discrawl wiretap --full-cache
discrawl wiretap --watch-every 2m
discrawl sync --source wiretap
```

Use `--dry-run` before unusual paths or broad cache archaeology. Use `--watch-every` for a local
import loop while the user scrolls Discord Desktop.

## DMs

```bash
discrawl dms
discrawl dms --with Molty --last 20
discrawl dms --with 1456464433768300635 --all
discrawl dms --search "launch checklist"
discrawl dms --with Molty --search "invoice"
discrawl search --dm "launch checklist"
discrawl tui --dm
```

`discrawl dms` lists one row per local DM conversation unless `--with` switches to message output.
Use `--search` for DM-only text search.

## Boundaries

- Wiretap reads local Discord Desktop artifacts only.
- Never extract credentials, use user tokens, call Discord as the user, or write to Discord
  application storage.
- Treat imported DMs as incomplete local cache evidence, not complete live DM history.
- Proven DMs live under the synthetic guild id `@me`.
- `@me` rows, wiretap sync state, and local DM media are never published to Git snapshots.
- Do not describe wiretap DMs as part of the shared Git-backed archive.
