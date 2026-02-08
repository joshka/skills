# Doc Steward

`doc-steward` helps you write, upgrade, and review developer docs with stronger contracts and less
maintenance drift.

This README is user-facing. It explains how to invoke the skill, which mode to choose, what outputs
to expect, and how to decide if a pass is done.

## Start Here

Front door (recommended):

```text
Use $doc-steward.
```

Direct mode entry (skip menu):

```text
Use $doc-steward in PR Narration mode as a contributor, focused on touched files, with a quick pass.
```

## Choose A Mode

1. [`Ratchet Mode`](#ratchet-mode): tighten existing docs while preserving local tone.
1. [`PR Narration Mode`](#pr-narration-mode): explain PR intent, contracts, risks, and doc deltas.
1. [`Bootstrap Mode`](#bootstrap-mode): create a practical first README and onboarding path.
1. [`Greenfield Full Coverage Approach`](#greenfield-full-coverage-approach): produce deep early
   coverage, including private logic.
1. [`Docstring / Rustdoc Mode`](#docstring--rustdoc-mode): align API/internal docs with behavior.
1. [`Recovery Mode`](#recovery-mode): repair stale or inconsistent docs in mixed-quality repos.
1. [`Bulk Update Mode`](#bulk-update-mode): standardize terminology and structure at scale.
1. [`Process Improvement Mode`](#process-improvement-mode): define repo/team docs standards.

### Ratchet Mode

Choose this when docs are mostly present and mostly correct, but quality is uneven. This is the
right option for incremental improvement during normal feature work without rewriting the whole docs
surface.

Run:

```text
Use $doc-steward in Ratchet mode as a contributor, focused on touched files, with a quick pass.
```

Outcome: targeted doc edits, drift fixes, and prioritized follow-ups in selected scope.

### PR Narration Mode

Choose this when reviewers will struggle to infer intent from diffs alone. It turns implementation
details into a concise explanation of model changes, constraints, and review risk.

Run:

```text
Use $doc-steward in PR Narration mode as a contributor, focused on touched files, with a quick pass.
```

Outcome: reviewer-ready narrative, contract summary, required doc deltas, and risk list.

### Bootstrap Mode

Choose this for new or under-documented repos where speed matters more than full depth. It is the
fastest way to get an onboarding path and useful baseline docs into place.

Run:

```text
Use $doc-steward in Bootstrap mode as a contributor, focused on touched files, with a quick pass.
```

Outcome: first README/guide with a usable quick path, assumptions, and next additions.

### Greenfield Full Coverage Approach

Choose this when you want deep documentation coverage early and do not want quality debt to
accumulate. This is the strongest option for new apps where internal/private complexity should be
documented from day one.

Run:

```text
Use $doc-steward with the Greenfield Full Coverage approach as a repo owner, repo-wide, with full detail.
```

Outcome: repo-wide coverage ledger, depth summary (see
[Depth Levels](references/doc-depth.md#depth-levels)), concrete rewrites, and unknown closure.

### Docstring / Rustdoc Mode

Choose this after interface or behavior changes when symbol-level docs are likely stale. It is best
when you need contract alignment at the API/function level more than README-level writing.

Run:

```text
Use $doc-steward in Docstring / Rustdoc mode as a repo owner, repo-wide, full detail, deep docs.
```

Outcome: symbol-level contract updates and alignment notes for docs/tests/call sites.

### Recovery Mode

Choose this when docs are mixed-quality, contradictory, or stale across areas of the repo. It
prioritizes remediation so you can improve quality in reviewable batches instead of a single large
rewrite.

Run:

```text
Use $doc-steward in Recovery mode as a repo owner, with repo-wide scope and full detail.
```

Outcome: prioritized remediation backlog, first fix batch, and remaining drift ledger.

### Bulk Update Mode

Choose this for broad consistency work across many files after terminology/style drift has
accumulated. It is for standardization passes, not first-time doc creation.

Run:

```text
Use $doc-steward in Bulk Update mode as a repo owner, with repo-wide scope and full detail.
```

Outcome: normalized terminology/structure and unresolved drift report.

### Process Improvement Mode

Choose this when your main goal is improving team/repo documentation process rather than editing
one document set. Use it to define standards, contributor expectations, and operational workflow.

Run:

```text
Use $doc-steward in Process Improvement mode as a repo owner, repo-wide, with a quick pass.
```

Outcome: standards proposal, AGENTS guidance draft, and adoption checklist.

## What To Expect After Invocation

When you run a one-liner, expected flow is:

1. The assistant asks you to choose a mode unless your one-liner already sets one.
1. It asks for profile only when needed (`use defaults` is valid).
1. It produces concrete outputs, not advisory-only commentary.
1. It ends with Unknown confirmation when Unknowns remain.

For `full` and Greenfield-style runs, expect:

1. Coverage ledger for modules, types, and functions in scope.
1. Depth summary (`L0`/`L1`/`L2`/`L3`) with judging criteria from
   [Depth Levels](references/doc-depth.md#depth-levels) and
   [Quality Bar](#quality-bar-how-to-judge-output).
1. Shallow-to-improved rewrite examples when depth is weak.

Depth levels at a glance:

1. `L0`: insufficient (usually signature/name restatement).
1. `L1`: basic purpose plus one concrete contract detail.
1. `L2`: good operational depth (assumptions, side effects, constraints).
1. `L3`: strong depth with edge/failure behavior and ordering/invariant rationale.

Use this summary for quick triage; use
[Depth Levels](references/doc-depth.md#depth-levels) for full judging criteria.

## Feedback Loop

Use short replies to guide iteration:

```text
use defaults
```

```text
owner, repo-wide, full, preserve-local
```

```text
Proceed and keep iterating until coverage and depth gates are satisfied.
```

```text
Resolve unknown items 1 and 3 now.
```

## Quality Bar (How To Judge Output)

Use this as acceptance criteria for a pass. If any item fails, request another pass.

1. Contract quality: docs explain purpose, constraints, invariants, side effects, and important
   edge/failure behavior.
1. Signal quality: comments do not just restate names/signatures.
1. Drift control: behavior and docs are updated together in the same change.
1. Unknown control: Unknown items are few, contextualized, and explicitly confirmed.

## References

- Skill contract: [SKILL.md](SKILL.md)
- Quick prompt variants: [references/quick-modes.md](references/quick-modes.md)
- Depth guide: [references/doc-depth.md](references/doc-depth.md)
- Review rubric: [references/doc-review-rubric.md](references/doc-review-rubric.md)
- Diataxis guide: [references/diataxis.md](references/diataxis.md)
- Improvement plan for this repo: [PLAN.md](PLAN.md)
