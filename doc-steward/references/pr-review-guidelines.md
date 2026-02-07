# PR Review Guidelines

Use this when reviewing a PR for documentation quality, narrative completeness, and drift risk.

## Review Priorities

1. Correctness: narrative matches actual behavior.
1. Contracts: invariants and assumptions are explicit.
1. Risk: failure modes and edge cases are acknowledged.
1. Drift: docs changed alongside behavior changes.
1. Operability: observability and debugging paths are clear.

## Reviewer Checks

- Does the PR description explain "why" and "what changed"?
- Are non-goals and tradeoffs explicit?
- Are public-facing behavior changes documented?
- Are references/examples still runnable and accurate?
- Are unknowns tagged as `Unknown` instead of guessed?

## Recommended Review Output

```text
Findings
1. <severity> <issue>

Documentation Deltas Required Before Merge
1. <file/section> <required change>

Open Questions
1. <question>
```

## Severity Hints

- High: behavior misunderstood or contract ambiguity likely to cause bugs.
- Medium: important missing context, but behavior still inferable.
- Low: polish/usability issues that do not change interpretation.
