# Doc Steward

`doc-steward` helps you write, upgrade, and review developer docs with clear contracts and low
maintenance drift.

## Start Here

Front door (recommended):

```text
Use $doc-steward.
```

Direct mode entry (skip menu):

```text
Use $doc-steward in PR Narration mode as a contributor, focused on touched files, with a quick pass.
```

## One-Liner Expectations

When you run a one-liner, expect this flow:

1. The assistant asks you to choose a mode, unless your one-liner already names one.
1. It asks for profile only if needed (`use defaults` is valid).
1. It produces concrete outputs, not advice only.
1. It ends with Unknown confirmation if any Unknowns remain.

For `full` and Greenfield-style runs, expect:

1. Coverage ledger for modules, types, and functions in scope.
1. Depth summary (`L0`/`L1`/`L2`/`L3`).
1. Shallow-to-improved rewrite examples when depth is weak.

## Choose A Mode

1. [`Ratchet Mode`](#ratchet-mode): improve existing docs while preserving local tone.
1. [`PR Narration Mode`](#pr-narration-mode): explain PR intent, contracts, risks, and doc deltas.
1. [`Bootstrap Mode`](#bootstrap-mode): create a usable first README and onboarding path.
1. [`Greenfield Full Coverage Approach`](#greenfield-full-coverage-approach): deep early docs,
   including private logic.
1. [`Docstring / Rustdoc Mode`](#docstring--rustdoc-mode): align API and implementation docs with
   behavior.
1. [`Recovery Mode`](#recovery-mode): repair stale or inconsistent docs in mixed-quality repos.
1. [`Bulk Update Mode`](#bulk-update-mode): standardize terminology and structure across many docs.
1. [`Process Improvement Mode`](#process-improvement-mode): define repo-level standards and docs
   workflow.

### Ratchet Mode

Use when docs mostly work and you want targeted improvements.
You should get: focused doc edits, drift fixes, and prioritized follow-ups.

### PR Narration Mode

Use for reviewer handoff when code context is implicit.
You should get: PR narrative, contract summary, doc deltas, and risk list.

### Bootstrap Mode

Use when a repo has little or no docs.
You should get: first README, quick-start path, assumptions, and next additions.

### Greenfield Full Coverage Approach

Use when you want full documentation depth early.
You should get: repo-wide coverage ledger, depth summary, concrete rewrites, and closure on
remaining unknowns.

### Docstring / Rustdoc Mode

Use after behavior/interface changes.
You should get: updated contracts on relevant symbols and doc/test alignment notes.

### Recovery Mode

Use when docs are mixed quality or stale.
You should get: prioritized backlog, first remediation batch, and remaining drift ledger.

### Bulk Update Mode

Use for larger consistency passes.
You should get: normalized terminology/structure and unresolved drift report.

### Process Improvement Mode

Use when setting repo/team documentation standards.
You should get: standards proposal, AGENTS guidance draft, and adoption checklist.

## Feedback Loop

Use short replies to control iteration:

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

## Constraints And Guardrails

1. Prioritize purpose, constraints, invariants, side effects, and edge behavior.
1. Avoid comments that only restate names/signatures.
1. Keep docs and behavior aligned in the same change.
1. Keep Unknown items bounded and explicitly confirmed.

## References

- Skill contract: [SKILL.md](SKILL.md)
- Quick prompt variants: [references/quick-modes.md](references/quick-modes.md)
- Depth guide: [references/doc-depth.md](references/doc-depth.md)
- Review rubric: [references/doc-review-rubric.md](references/doc-review-rubric.md)
- Diataxis guide: [references/diataxis.md](references/diataxis.md)
- Improvement plan for this repo: [PLAN.md](PLAN.md)
