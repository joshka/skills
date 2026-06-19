---
name: discrawl-remote-cloud
description: Use when Codex needs to read or diagnose Discrawl Cloudflare remote archive status, archive lists, identity with whoami, or explain remote read-only mode versus local SQLite archive behavior.
---

# Discrawl Remote Cloud

Use remote commands for read-only Cloudflare archive status and identity checks. The Worker is
deployed separately from Discrawl; Discrawl stores endpoint, archive, and token configuration.

## Commands

```bash
discrawl remote status
discrawl remote archives
discrawl remote whoami
discrawl whoami
discrawl subscribe-cloud --help
```

Remote commands require `[remote]` config with `mode = "cloud"`, `endpoint`, `archive`, and a
token in `remote.token_env`, defaulting to `DISCRAWL_REMOTE_TOKEN`.

## Rules

- Treat remote mode as read-only archive access.
- Do not create or mutate the local SQLite archive when answering remote status questions.
- Do not deploy or modify the Cloudflare Worker from Discrawl; that lives in the remote service
  repository.
- Use `whoami` to verify identity before making access-control claims.
- Keep local Git snapshot, local SQLite, and Cloudflare remote behavior distinct in explanations.
