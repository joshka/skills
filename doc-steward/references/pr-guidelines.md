# PR Guidelines

Use this when writing PR descriptions for code changes that need clear technical narration.

## Objective

Make the change understandable to a reviewer who has not read the full diff.

## Required Sections

1. Problem: what issue or gap this change addresses.
1. Mental model: how the updated system works at a high level.
1. Non-goals: what this PR intentionally does not solve.
1. Tradeoffs: meaningful alternatives and why they were not chosen.
1. Architecture impact: modules, APIs, or flows changed.
1. Observability: logs/metrics/traces impacted or added.
1. Tests: what was validated and what remains untested.
1. Documentation deltas: docs that were updated or still need updates.

## Writing Rules

- Describe behavior and intent, not line-by-line code edits.
- Keep speculative statements labeled (`Inferred` or `Unknown`).
- Link critical claims to evidence (tests, call sites, runtime behavior).
- Keep scope boundaries explicit.

## Quality Checklist

- A reviewer can answer "why now?" and "what changed for users/operators?"
- Risks and fallback behavior are described.
- Follow-up work is separated from this PR scope.
- Any contract changes are called out explicitly.

## Pasteable PR Skeleton

```text
## Problem

## Mental Model

## Non-Goals

## Tradeoffs

## Architecture Impact

## Observability

## Tests

## Documentation Deltas
```
