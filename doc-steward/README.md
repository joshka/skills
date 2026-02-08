# Doc Steward

`doc-steward` helps you create, upgrade, and review developer documentation with high signal and low
noise.

It is built for real maintenance work: preserving local tone, reducing doc drift, and making
changes easier to review.

## Start Here

Use the front door:

```text
Use $doc-steward.
```

Then pick the goal that matches your situation.

## Pick The Right Mode

1. `Ratchet Mode`: improve existing docs without changing project voice.
1. `PR Narration Mode`: explain a PR's intent, contracts, risks, and required doc updates.
1. `Bootstrap Mode`: create a practical first README and onboarding path.
1. `Greenfield Full Coverage Approach`: document a new codebase deeply, including private logic.
1. `Docstring / Rustdoc Mode`: update API and implementation docs after behavior changes.
1. `Recovery Mode`: repair stale or inconsistent docs in mixed-quality repos.
1. `Bulk Update Mode`: standardize terminology and structure across many docs.
1. `Process Improvement Mode`: define repo-level docs standards and contributor workflow.

## What You Should Get

The output should be concrete, not advisory only.

1. A docs patch (or exact file edits) for the selected scope.
1. A drift list (what was stale/inconsistent and what was fixed).
1. A follow-up list with prioritized next steps.
1. An Unknown confirmation step at the end.

For `full` and Greenfield-style runs, expect:

1. Coverage ledger for modules, types, and functions in scope.
1. Depth check summary (`L0`/`L1`/`L2`/`L3`).
1. Explicit shallow-to-improved rewrite examples when depth is weak.

## Interaction Pattern

Give minimal context and iterate:

1. Choose mode.
1. Keep defaults or set a profile.
1. Review outputs.
1. Ask for another pass until coverage/depth goals are met.

Useful replies:

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

## Greenfield Full Coverage

Use this when you want deep docs early and do not want to defer quality.

Definition of done:

1. README and quick path are usable from a clean checkout.
1. Non-test module/type/function docs meet coverage targets.
1. Non-trivial code has meaningful contract depth (not signature restatement).
1. Remaining unknowns are explicitly accepted or resolved.
1. Validation checks pass for the target repo.

## Constraints And Guardrails

1. Docs should explain purpose, constraints, invariants, side effects, and edge behavior.
1. Avoid comments that only restate names or signatures.
1. Treat docs as part of the contract; keep behavior and docs aligned in the same change.
1. Keep Unknown items bounded and explicitly confirmed.

## References

- Skill contract: [SKILL.md](SKILL.md)
- Quick prompt variants: [references/quick-modes.md](references/quick-modes.md)
- Depth guide: [references/doc-depth.md](references/doc-depth.md)
- Review rubric: [references/doc-review-rubric.md](references/doc-review-rubric.md)
- Diataxis guide: [references/diataxis.md](references/diataxis.md)
- Improvement plan for this repo: [PLAN.md](PLAN.md)
