# Doc Steward Improvement Plan

This plan tracks upgrades that make `doc-steward` more effective across mature, greenfield,
and mixed-quality codebases.

This `PLAN.md` workflow is specific to this repository and is not a required pattern for other repos.

## Goals

- Ratchet documentation quality in projects with existing docs.
- Enable fast bootstrap docs for projects with little/no documentation.
- Improve PR review narration when code context is implicit.
- Support bulk documentation changes with consistency and drift control.

## High-Leverage Improvements

### 1. Mode-first operation

- Keep Goal Picker as the default entry point.
- Ensure each mode has clear outputs and acceptance checks.
- Minimize required upfront context; ask only blocking questions.
- In Process Improvement mode, proactively suggest reusable repo standards.

### 2. Evidence/confidence policy

- Label claims as `Confirmed`, `Inferred`, or `Unknown`.
- Separate proven behavior from assumptions.
- Flag unknowns with exact follow-up needed.

### 3. Tone-lock for existing projects

- Sample local docs before writing.
- Preserve terminology and structure where possible.
- Normalize only when inconsistency hurts comprehension.

### 4. Strong PR narration outputs

- Standard PR narrative sections.
- Explicit contract/invariant extraction.
- Required doc deltas and review risks.

### 5. Recovery workflow for weak docs

- Audit -> score -> prioritize -> patch in small batches.
- Keep a rolling remediation backlog.
- Re-verify after each batch.

### 6. Bulk-update control points

- Apply glossary/term rules first.
- Track changed artifacts and unresolved drift.
- Emit end-of-pass risk report.

## Skill Structure Changes

### `doc-steward/SKILL.md`

- Add Goal Picker default mode.
- Add mode-to-output mapping.
- Add compact pasteable starter prompts.

### `doc-steward/references/`

Add:

- `pr-guidelines.md`
- `pr-review-guidelines.md`
- `docs-bootstrap.md`
- `docs-remediation.md`
- `bulk-doc-changes.md`
- `tone-and-style-lock.md`

Revise:

- `doc-review-rubric.md` with evidence/confidence checks.
- `readme.md` with bootstrap and upgrade variants.
- `comment-steward-prompts.md` with PR narration/recovery prompts.

## AGENTS.md Integration Plan

For repositories that adopt this skill, update their `AGENTS.md` to include:

- Markdown standards (`markdownlint-cli2`, 100-char line limit unless repo overrides).
- "Docs-as-contract" guidance for docstrings/comments.
- Instruction to use `doc-steward` for docs drafting/review/bulk updates.
- A pointer to your skills repo:
  `https://github.com/joshka/skills/tree/main/doc-steward`

## Delivery Phases

### Phase 1

- Land markdownlint config and mode-first skill contract.
- Publish user-facing README usage guidance.

### Phase 2

- Add missing reference docs and templates.
- Validate mode outputs on representative projects.

Phase 2 status in this repo:

- Implemented reference docs and template updates.
- Added short one-line mode invocations and first-run profile dimensions.
- Next validation target: `~/local/jk` (greenfield, minimal/no docs).

### Phase 3

- Add AGENTS.md snippets/templates for downstream repositories.
- Refine defaults from real PR review usage.

## Acceptance Criteria

- One-command invocation works via Goal Picker in normal sessions.
- Each mode produces a concrete, review-ready deliverable.
- PR narration output is clear enough for reviewers with limited context.
- Bootstrap mode creates immediately usable starter docs.
- Bulk mode preserves consistency and reports unresolved drift.

## Session Status Update (2026-02-07)

Implemented in this session:

1. Expanded documentation depth guidance from method-only to module/type/function coverage.
2. Added a required resource-loading contract in `SKILL.md` so Greenfield and Docstring modes
   must load `references/doc-depth.md`.
3. Added `references/doc-depth.md` with quality floor rules, depth templates, and a coverage
   ledger shape.
4. Updated `references/doc-review-rubric.md` to check module/type/function depth and reject
   low-value docs that only restate signatures.
5. Removed compatibility pointer usage:
   - deleted `references/method-doc-depth.md`
   - removed all references to that file from `SKILL.md`
6. Improved front-door mode selection clarity:
   - reordered Goal Picker options
   - added concise "use when/includes" guidance
   - clarified Bootstrap vs Greenfield differences
7. Improved optional context prompts:
   - converted to short phrase-style lines with defaults
   - removed `docs-only` from optional constraints (docs-only is already default)

Validation evidence captured:

1. Reviewed session log
   `~/.codex/sessions/2026/02/07/rollout-2026-02-07T14-41-13-019c3a44-3274-7441-bc2a-8f61ad7e385d.jsonl`.
2. Confirmed prior run selected Greenfield (`option 4`) but did not load `doc-depth.md`.
3. Added explicit loading requirements to close that gap.

Next validation run (`~/local/jk`, front-door only):

1. Invoke via `$doc-steward` and choose Greenfield full-coverage path from the menu.
2. Confirm the run explicitly loads `references/doc-depth.md`.
3. Confirm output includes module/type/function depth checks and a coverage ledger.
4. Confirm docs avoid low-signal comments and include behavior, constraints, and edge cases.
5. Record remaining `Unknown` items and run the required end-of-pass confirmation.

## Validation and Measurement Protocol

Pass criteria were captured in session log
`~/.codex/sessions/2026/02/07/rollout-2026-02-07T11-34-37-019c3999-5be4-7fb0-bf34-25823aa7500c.jsonl`
and are retained here to avoid compaction loss.

### Front-door test flow

1. Run from target repo root with `Use $doc-steward.`.
2. Choose the Greenfield full-coverage option from Goal Picker.
3. Calibrate as `owner`, `repo-wide`, `full`, `preserve-local`.
4. Continue until coverage and depth gates are satisfied.

### Pass criteria

1. Goal menu shows clear option descriptions and Bootstrap vs Greenfield distinction.
2. Coverage ledger is included for modules, types, and functions in non-test source.
3. Function coverage is at least 95% repo-wide and 100% for touched files in full-coverage runs.
4. Unknowns are bounded, listed with context, and explicitly accepted or resolved.
5. Docs include purpose plus at least one of: constraints, invariants, edge/failure behavior.
6. Validation commands pass (`cargo check`, markdown lint for repo markdown files).

### Measurement commands

```bash
jj --no-pager diff -r @- -r @ --name-only
cargo check
markdownlint-cli2 "**/*.md"
```

Then compute a coverage ledger over changed non-test `src/**/*.rs`:

1. Module coverage: file has crate/module doc (`//!`) near the top.
2. Type coverage: `struct`/`enum`/`trait` declarations have adjacent `///`.
3. Function coverage: function/method declarations have adjacent `///`, excluding test-only items.
4. Depth check: flag low-signal one-liners that only restate the symbol name.

### Latest `~/local/jk` evaluation (change `nsklsruomkrl`)

1. Changed non-test source files: `31`.
2. Module docs: `31/31` (`100.0%`).
3. Type docs: `26/26` (`100.0%`).
4. Function docs: `148/150` (`98.7%`), missing:
   - `src/alias/normalize.rs:116`
   - `src/commands/spec.rs:342`
5. Depth heuristic: `92/148` documented functions (`62.2%`) flagged as potentially shallow.
6. Validation: `cargo check` passed and `markdownlint-cli2 "**/*.md"` passed.

Interpretation:

1. Coverage is strong but not full for changed files due to 2 missing helper docs.
2. Depth quality is below target for Greenfield full-coverage expectations.
3. Next run should raise depth and close the remaining uncovered functions.

### Depth gap detail from `~/local/jk`

Patterns repeatedly observed in shallow docs:

1. Dispatch hubs documented only as "Handles ..." without precedence/order semantics.
2. State transition methods documented without reset invariants or side-effect boundaries.
3. Fallback resolution methods documented without preference order and fallback rationale.
4. Safety preview helpers documented without explicit safety model and `None` semantics.

Required preferred shape for these patterns:

1. Dispatch hubs: purpose, first-match precedence, state transitions, and no-op behavior.
2. State transitions: per-variant effects, reset behavior, and delegated responsibilities.
3. Fallback logic: preferred source, fallback source, and behavior guarantees.
4. Safety helpers: what is considered safe, fallback behavior, and caller obligations.

Implementation updates completed:

1. Added depth levels (`L0` to `L3`) and complexity triggers in
   `references/doc-depth.md`.
2. Added concrete shallow-to-improved Rustdoc examples in `references/doc-depth.md`.
3. Updated `references/doc-review-rubric.md` to require depth scoring and shallow rewrites.
4. Updated `SKILL.md` and `README.md` to require depth score summaries in full runs.
