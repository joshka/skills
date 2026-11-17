# Skills Workspace

This repository is my local workspace for Codex skills and related references.

It is not a single-skill repo. It contains `doc-steward` today and is intended to include other
skills I use over time.

## What Lives Here

- Skill folders with a `SKILL.md` contract.
- Supporting references/templates used by those skills.
- Lightweight repo-level configuration for documentation quality.

## Current Skills

- `doc-steward`: documentation drafting, review, and maintenance workflows.
- `lock-actions`: moves a repo from SHA-pinned GitHub Actions to a workflow lockfile
  (`actions.lock`), with a CI verify job and a maintainer guide.
- `practice-rust-guidance`: routes Rust library and app work to practice guidance entry points.
- `thread-closeout`: reviewable Codex thread closeouts with validation, failures, follow-ups, and
  improvement routing.

## Layout

```text
.
├── README.md
├── .markdownlint-cli2.yaml
├── doc-steward/
│   ├── SKILL.md
│   ├── README.md
│   ├── PLAN.md
│   └── references/
├── lock-actions/
│   ├── SKILL.md
│   ├── agents/
│   └── assets/
├── practice-rust-guidance/
│   ├── SKILL.md
│   └── agents/
├── thread-closeout/
│   ├── SKILL.md
│   └── agents/
└── ... (additional skills over time)
```

## Usage

- Run skills from this workspace during Codex sessions.
- Keep each skill self-contained with clear instructions and references.
- Add new skills as separate top-level folders following the same pattern.

## Installing a Skill

Each skill folder works with both Codex and Claude Code. Link it into each tool's skills directory
so edits here take effect without copying:

```bash
git clone https://github.com/joshka/skills ~/local/skills   # or jj git clone
ln -s ~/local/skills/<skill> ~/.codex/skills/<skill>
ln -s ~/local/skills/<skill> ~/.claude/skills/<skill>
```

Start a new session afterwards; skills are discovered at startup.

## Using lock-actions

Prerequisites: an authenticated `gh`, and the extension at the version the skill records:

```bash
gh extension install github/gh-actions-lock --pin v0.4.1
```

From the repo you want to lock, ask the agent to "lock this repo's GitHub Actions" (Codex:
`$lock-actions`; Claude Code: `/lock-actions`). It works through one repo at a time:

1. Surveys the repo's CI against the conventions in the skill and reports any gaps. Big gaps stop
   the run with a recommendation.
1. Generates `.github/workflows/actions.lock` and checks that no locked commit changed.
1. Adds a `lockfile` job to the main check workflow and to its `required` job.
1. Adds `docs/github-actions.md` and an `AGENTS.md` pointer.
1. Opens a PR that explains every ref change.

It never pushes to the default branch, enables repo policies, or accepts moved commits. Review the
PR, especially the "Ref changes" section, before merging. After merge, optionally turn on the
"Require lockfile" policy under Settings → Actions → Policies.

When `gh-actions-lock` releases a new version, the skill notices and proposes updates to itself
rather than upgrading silently.
