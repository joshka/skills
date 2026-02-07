# Documentation Review Rubric

Use this checklist to review existing docs or a doc diff. Prefer concrete edits over vague
feedback.

## Evidence And Confidence

- Label key claims as `Confirmed`, `Inferred`, or `Unknown` when uncertainty exists.
- `Confirmed` claims should map to code/tests/runtime behavior.
- `Inferred` claims should state what evidence supports the inference.
- `Unknown` claims should include the missing evidence needed to confirm.
- Keep `Unknown` count intentionally low for the current pass scope.

## Unknown Confirmation

- Remaining `Unknown` items are listed as numbered entries with file/section context.
- Review output ends with a confirmation prompt:
  - "Confirm whether to keep all remaining Unknowns, or list item numbers to resolve now."

## Correctness

- Matches the code: names, flags, defaults, error cases, and constraints.
- Examples compile/run (or are clearly labeled as pseudo-code).
- Version/feature assumptions are explicit (MSRV, feature flags, platform support).
- Claims are supportable from code/tests/usages; call out anything speculative.

## Completeness

- States prerequisites and environment assumptions.
- Defines terms once; uses the same terms consistently.
- Includes the next step: what the reader should do after finishing this section.
- Covers failure modes that matter (common misconfigurations, bad inputs).

## Documentation Depth

- For each module in scope, docs include purpose, boundaries, and invariants.
- For each type in scope, docs include role, semantics, and invariants.
- For each method/function in scope, docs include purpose, assumptions, and side effects.
- Non-trivial methods/functions include input/output semantics and edge/failure behavior.
- Comments explain intent and constraints, not just line-by-line mechanics.
- A coverage ledger is provided: documented vs total modules/types/functions in scope.
- Remaining undocumented items are listed with exact file paths and line anchors.
- Reject docs that only restate names/signatures.

Depth scoring checkpoints:

1. Score samples using `L0` to `L3` from `doc-depth.md`.
1. Treat `L0` as failing and block finalization until upgraded.
1. In `full` passes, keep `L0` at `0` and keep non-trivial code at `L2+`.
1. Include at least 3 concrete shallow-to-improved rewrites in review output when depth issues exist.

## Clarity

- Starts with a one-sentence purpose and intended audience.
- Uses headings that match reader questions (not internal code structure).
- Uses short paragraphs and lists for procedural content.
- Avoids ambiguous pronouns ("it", "this") where they could refer to multiple things.

## Usability

- Commands include enough context to run (paths, flags, required env vars).
- Shows expected output (or what success looks like).
- Keeps examples minimal and focused on one concept at a time.
- Avoids copy/paste traps (ellipses in commands, missing quotes, hidden state).

## Maintainability (Drift Prevention)

- Treats docstrings/comments as contracts: if behavior changes, update docs and tests together.
- Avoids duplicating knowledge in multiple places; links to the source of truth instead.
- Explains the "why" only when it is provable (and labels unknowns as TODOs).
- Removes or rewrites low-value commentary that only restates the code.

## Presentation

- Markdown is lintable and consistent (see `markdown-style.md`).
- Code blocks specify a language.
- Links are scoped and stable (avoid "click here").
