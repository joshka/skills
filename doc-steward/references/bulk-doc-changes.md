# Bulk Documentation Changes

Use this for large documentation passes touching many files.

## Objective

Improve consistency and correctness without introducing new drift.

## Control Strategy

1. Define glossary and terminology rules before editing.
1. Split work into bounded batches (by module or doc type).
1. Apply one consistency objective per batch.
1. Lint and verify examples after each batch.
1. Publish unresolved drift at the end.

## Recommended Batch Size

- Keep each batch small enough for focused review.
- Prefer 5-15 files per batch unless changes are mechanical.

## Required Outputs

- Batch summary: what changed and why.
- Drift ledger: known unresolved inconsistencies.
- Follow-up queue: highest-impact next fixes.

## Risk Controls

- Avoid rewriting meaning while normalizing style.
- Do not duplicate source-of-truth details across many files.
- Prefer links to canonical references for volatile details.
