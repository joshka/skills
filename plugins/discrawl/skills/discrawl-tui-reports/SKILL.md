---
name: discrawl-tui-reports
description: Use when Codex needs to browse Discrawl archives with the TUI, emit JSON TUI rows for agent workflows, generate digests or reports, or update activity README blocks for backup repositories.
---

# Discrawl TUI Reports

Use the TUI for interactive archive browsing and `--json` TUI output when an agent needs the same
row shape without opening an interactive terminal.

## TUI

```bash
discrawl tui
discrawl tui --guild 123456789012345678 --channel general
discrawl tui --guilds 123,456 --author 1456464433768300635
discrawl tui --dm
discrawl --json tui --limit 50
```

The explorer is read-only. It groups channels, people, or threads, lists matching messages, and
shows selected message detail, attachments, replies, and thread context.

## Reports

```bash
discrawl report
discrawl report --readme path/to/discord-backup/README.md
discrawl digest --help
```

Use `report` for deterministic archive activity blocks: latest update, latest message, totals,
and day, week, and month activity.

## Rules

- Use `--json` when Codex needs stable rows instead of an interactive terminal.
- Use `tui --dm` only for local wiretap DMs under `@me`.
- Do not use report totals for filtered publish snapshots unless the report source is explicitly
  scoped to the same filtered data.
- When docs or help are needed for `digest`, inspect `discrawl digest --help` in the active
  install before giving exact flags.
