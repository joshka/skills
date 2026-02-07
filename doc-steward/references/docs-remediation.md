# Docs Remediation Guide

Use this for repositories with mixed-quality or stale documentation.

## Objective

Raise documentation quality through prioritized, reviewable increments.

## Remediation Workflow

1. Inventory docs by type (README, guides, API docs, comments).
1. Score each artifact for correctness, completeness, and drift risk.
1. Prioritize by impact and maintenance cost.
1. Patch highest-impact items first.
1. Re-score and update backlog after each batch.

## Prioritization Heuristics

- User/runtime impact first (setup, safety, operational behavior).
- Public API contract docs next.
- Internal explanation docs and low-traffic pages last.

## Backlog Item Template

```text
Title: <artifact + issue>
Impact: <high|medium|low>
Evidence: <test/call-site/runtime/doc mismatch>
Fix: <exact section or file to update>
Owner: <optional>
```

## Exit Criteria

- High-impact drift items are resolved.
- Critical setup/run docs are accurate.
- Remaining debt is explicit and prioritized.
- Remaining unknowns are explicitly accepted via a final confirmation step.
