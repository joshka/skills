# Repository Guidelines

## Project Structure & Module Organization

This repository is a skills workspace, not an application codebase. Keep top-level folders
focused on individual skills.

- `doc-steward/`: active documentation skill.
- `doc-steward/SKILL.md`: canonical behavior and workflow contract.
- `doc-steward/README.md`: day-to-day usage guidance.
- `doc-steward/PLAN.md`: local improvement plan for this repo.
- `doc-steward/references/`: reusable prompts, rubrics, and style guides.
- `.markdownlint-cli2.yaml`: Markdown lint configuration.

When adding a new skill, follow the same pattern: `<skill-name>/SKILL.md` plus targeted references.

## Build, Test, and Development Commands

Use lightweight validation commands:

```bash
markdownlint-cli2 "**/*.md"
```

Lints Markdown in the repository.

```bash
jj --no-pager status
jj --no-pager diff --git
```

Shows working-copy changes and reviewable diffs.

```bash
rg --files
```

Fast file discovery for skill and reference updates.

## Coding Style & Naming Conventions

- Markdown-first repository: no broad refactors for style-only edits.
- Wrap prose at 100 characters unless a repo-specific config overrides it.
- Keep a blank line after headings and around lists, blockquotes, and code blocks.
- Use fenced code blocks with a language tag; Rustdoc may omit language when Rust is implied.
- For ordered lists, always use `1.` style markers, never `1)`.
- Keep docs drift-free when behavior changes.
- Prefer explaining constraints, invariants, and rationale over restating code.
- Use kebab-case for skill folder names (example: `doc-steward`).

## Skill Reference

Use `doc-steward` for documentation planning, drafting, review, and remediation.

- Local path: `doc-steward/SKILL.md`
- Canonical repo reference:
  `https://github.com/joshka/skills/tree/main/doc-steward`

## Testing Guidelines

There is no unit-test framework in this repository. Validation is documentation quality-focused.

- Run `markdownlint-cli2 "**/*.md"` before submitting changes.
- Verify example commands and paths in docs are real and copy/pasteable.
- Confirm new references linked from `SKILL.md` exist and are scoped to the skill.

## Commit & Pull Request Guidelines

- Use `jj` workflows for change review and commit management.
- Write commit messages in imperative mood with a short summary line (about 50 chars).
- Keep changes small and coherent; avoid mixing unrelated skill updates.
- PR descriptions should explain intent, key files changed, and follow-up work.
- When changing skill behavior, update both `SKILL.md` and user-facing docs in the same change.

## Guidance For Downstream Repos

When updating another repo's `AGENTS.md`, follow that repo's conventions and include:

- A short "Documentation Standards" section.
- Markdown linting command and configured line length.
- A docs-as-contract note for docstrings/comments.
- A pointer to the `doc-steward` skill for docs work.
