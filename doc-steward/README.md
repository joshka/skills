# Doc Steward

`doc-steward` helps you write, review, and maintain developer documentation without changing runtime
behavior. It is designed for day-to-day use in active codebases.

## What You Use It For

- Improve existing docs while keeping project tone and vocabulary.
- Bootstrap docs for new projects with minimal overhead.
- Review PRs and narrate design intent when changes are under-documented.
- Run bulk documentation updates with consistency controls.
- Keep docstrings/comments aligned with behavior as code evolves.

## Start Here

If you are new to the skill, start with one of these one-liners:

```text
Use $doc-steward.
```

```text
Use $doc-steward with the Greenfield Full Coverage approach as a repo owner, repo-wide, with full detail.
```

```text
Use $doc-steward in PR Narration mode as a contributor, focused on touched files, with a quick pass.
```

```text
Use $doc-steward in Docstring / Rustdoc mode as a repo owner, repo-wide, full detail, deep docs.
```

## Daily Workflow

1. Start with a goal.
1. Pick a profile (role + reach + depth).
1. Let the skill pick a mode, produce outputs, then refine.

Minimal input that works well:

- Goal: what outcome you want.
- Role: `contributor` or `owner`.
- Reach: `touched-files` or `repo-wide`.
- Depth: `quick` or `full`.

Unknown policy:

- Keep unknowns low; resolve to `Confirmed` when possible.
- End each pass with an Unknown confirmation prompt:
  - "Confirm whether to keep all remaining Unknowns, or list items to resolve now."

## Goal Picker (Default)

If you ask for `doc-steward` without selecting a mode, the skill should ask one question:

- "What do you want to achieve?"

Then map to one mode:

1. Ratchet docs quality in an existing project.
   Use when docs mostly exist and need focused improvements.
1. Review/narrate a PR and document design intent.
   Use during PR author/reviewer handoff.
1. Create first docs for a new project.
   Use when you need minimum viable docs fast.
1. Fully document a new app, including private methods.
   Use when you want deep module/type/function coverage from the start.
1. Update API docs/docstrings for changed behavior and internals.
   Use when interfaces or behavior changed.
1. Repair mixed-quality docs in an existing project.
   Use when docs are stale, inconsistent, or fragmented.
1. Run a bulk docs pass for consistency/drift.
   Use for broad terminology/style normalization.
1. Improve repo-wide docs process and standards.
   Use when setting team/repo documentation defaults.

Bootstrap vs Greenfield:

- Bootstrap gives you fast baseline docs (README, quick start, usage, config, troubleshooting).
- Greenfield Full Coverage adds deep module/type/function docs, coverage targets, and a coverage
  ledger.

Optional context (skip any item to use defaults):

Scope (default `touched-files`): `PR` changed files only; `files` exact files you list; `module`
one subsystem across related files; `path` everything under a folder path.

Audience (default `maintainer`): `maintainer` future code maintainers; `reviewer` PR reviewers
with limited context; `operator` people running/troubleshooting the system; `user` end users or
contributors following setup/usage docs.

Constraints (default repo style and lint rules): `style rules` preserve local
voice/terminology/conventions; `lint requirements` enforce configured lint checks (for example
`markdownlint-cli2`).

## Modes

### Ratchet Mode

Use for mature projects with usable docs.

- Tighten clarity and correctness.
- Preserve project tone.
- Fix drift in touched areas.

### Bootstrap Mode

Use for new or undocumented projects.

- Create README + quick start + usage skeleton.
- Add minimal architecture/context notes.
- Capture assumptions and unknowns clearly.

### PR Narration Mode

Use during PR authoring/review.

- Produce a clear PR narrative.
- Extract contracts/invariants from code.
- Identify doc deltas and review risks.

### Recovery Mode

Use for mixed-quality or stale docs.

- Audit current docs.
- Produce prioritized remediation backlog.
- Ship small, reviewable improvements.

### Bulk Update Mode

Use for large documentation sweeps.

- Apply shared glossary and terminology rules.
- Keep sections aligned across files.
- Report remaining drift/unknowns.

### Docstring / Rustdoc Mode

Use when changing public interfaces or complex logic.

- Update docstrings as contracts.
- Prioritize invariants, constraints, and edge cases.
- Keep examples runnable when feasible.

### Process Improvement Mode

Use when you want reusable documentation standards for a repo/team.

- Propose markdownlint setup (`markdownlint-cli2`, 100-char lines unless overridden).
- Recommend repo-native planning/documentation workflows instead of one fixed structure.
- In this repo specifically, we track skill evolution in `doc-steward/PLAN.md`.
- Draft `AGENTS.md` standards and skill pointers for downstream reuse.

## Quick Invocations

Use one-line prompts instead of large starter blocks:

```text
Use $doc-steward in Bootstrap mode as a contributor, focused on touched files, with a quick pass.
```

```text
Use $doc-steward in PR Narration mode as a contributor, focused on touched files, with a quick pass.
```

```text
Use $doc-steward in Recovery mode as a repo owner, with repo-wide scope and full detail.
```

```text
Use $doc-steward in Process Improvement mode as a repo owner, repo-wide, with a quick pass.
```

```text
Use $doc-steward with the Greenfield Full Coverage approach as a repo owner, repo-wide, with full detail.
```

For more short variants, see `doc-steward/references/quick-modes.md`.

## Repeatable Approaches

### Greenfield Full Coverage Approach

Use this when a new app should be thoroughly documented, including private methods.

Run this sequence:

1. `Bootstrap Mode` as `owner`, `repo-wide`, `full`.
1. `Docstring / Rustdoc Mode` as `owner`, `repo-wide`, `full`.
1. `Recovery Mode` as `owner`, `repo-wide`, `full`.
1. Apply the first remediation batch.
1. End with explicit Unknown confirmation.

Before drafting, load:

1. `references/doc-depth.md`
1. `references/docs-bootstrap.md`
1. `references/rustdoc.md`
1. `references/docs-remediation.md`
1. `references/doc-review-rubric.md`

Coverage policy:

- Modules: document purpose, boundaries, and invariants.
- Types: document role, semantics, and invariants.
- Public methods/functions: full contract docs.
- Private methods/functions: document purpose, assumptions, invariants, and side effects.
- Non-trivial methods/functions: include edge cases and failure behavior.

Coverage targets:

- `touched-files`: 100% non-test module/type/function documentation coverage.
- `repo-wide`: at least 95% non-test module/type/function documentation coverage.
- If below target, run another documentation pass before finalizing.

Quality floor:

- Avoid docs that only restate names/signatures.
- Prefer contract, constraints, and intent over implementation narration.
- In `full` runs, use depth levels from `references/doc-depth.md` and eliminate `L0` items.

Definition of done:

1. README and quick start are runnable from a clean checkout.
1. Public methods/functions are documented.
1. Private methods/functions are documented to policy.
1. Remaining unknowns are explicitly accepted or resolved.
1. Markdown lint passes.
1. Coverage ledger is included (modules/types/functions documented/total, plus remaining list).
1. Depth score summary is included (`L0`/`L1`/`L2`/`L3`) with rewrites for shallow docs.

## Examples

### Greenfield Project (Generic)

Ask it to generate a first README from current code/tests/commands and mark unknowns explicitly.

```text
Use $doc-steward in Bootstrap mode as a contributor, focused on touched files, with a quick pass.
```

Expected output:

1. README draft with quick start, usage, config, and troubleshooting.
1. Explicit `Unknown` markers where behavior is not provable yet.
1. Short follow-up list of highest-value next doc additions.
1. Final confirmation step for remaining unknowns (explicit acceptance required).

### PR Review Narration (Contributor Workflow)

Ask it to explain the change, list hidden contracts, and call out documentation deltas before merge.

```text
Use $doc-steward in PR Narration mode as a contributor, focused on touched files, with a quick pass.
```

Expected output:

1. Reviewer-friendly PR narrative.
1. Required docs updates by file/section.
1. Top risks and open questions.
1. Final confirmation step for remaining unknowns (explicit acceptance required).

### Repo Quality Pass (Owner Workflow)

Ask it to audit docs drift, prioritize fixes, and apply the first small remediation batch.

```text
Use $doc-steward in Recovery mode as a repo owner, with repo-wide scope and full detail.
```

Expected output:

1. Prioritized remediation backlog.
1. First batch of reviewable doc fixes.
1. Remaining drift ledger and recommended next batch.
1. Final confirmation step for remaining unknowns (explicit acceptance required).

### Process Standards Upgrade (Owner Workflow)

Ask it to draft AGENTS.md standards and recommend repo-native documentation workflow updates.

```text
Use $doc-steward in Process Improvement mode as a repo owner, repo-wide, with a quick pass.
```

Expected output:

1. Proposed standards update text.
1. Suggested skill usage guidance for contributors.
1. Minimal adoption checklist.

## Conventions

- Treat docs as part of the public API.
- Prefer "why/constraints/invariants" over restating implementation.
- Never invent rationale. Use Confirmed/Inferred/Unknown confidence tags when needed.
- Keep Markdown lintable (`markdownlint-cli2`) and consistent.

## References

- Skill contract: `doc-steward/SKILL.md`
- Improvement roadmap: `doc-steward/PLAN.md`
- Review checklist: `doc-steward/references/doc-review-rubric.md`
- Diataxis guide: `doc-steward/references/diataxis.md`
- Depth guide: `doc-steward/references/doc-depth.md`
