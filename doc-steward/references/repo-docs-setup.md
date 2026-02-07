# Reusable Repo Docs Setup

Use this checklist when asked to improve documentation process, not just individual files.

## Baseline Recommendations

- Add `markdownlint-cli2` configuration with a 100 character line limit.
- Keep a user-facing `README.md` for daily usage.
- Follow the target repo's existing planning/documentation conventions.
- If a roadmap/planning artifact exists, keep it updated in the repo's preferred format.
- Add documentation standards in `AGENTS.md`.
- Point `AGENTS.md` to the canonical `doc-steward` skill location.

## Suggested `AGENTS.md` Additions

Include a compact section with:

- Markdown lint command and configured line length.
- Docs-as-contract expectation for docstrings/comments.
- Guidance to use `doc-steward` for drafting/review/remediation.
- Link to canonical skill repo:
  `https://github.com/joshka/skills/tree/main/doc-steward`

## Small Pasteable Prompt

```text
Use $doc-steward in Process Improvement mode as a repo owner, repo-wide, with a quick pass.
```
