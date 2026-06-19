---
name: practice-rust-guidance
description: Use when Codex is writing, reviewing, documenting, or planning Rust libraries or applications and needs Josh's `/Users/joshka/local/practice` guidance entry points for Rust maintainability, Rustdoc, documentation workflow, examples, tests, API boundaries, errors, observability, change shape, or jj workflow. Use this as a router into practice guidance; do not use it for generic Rust syntax questions or tasks where the target repo already has more specific local guidance.
---

# Practice Rust Guidance

Use this skill to choose the smallest useful entry point in
`/Users/joshka/local/practice`. Do not load the whole practice repo by default.
Read the target repo's `AGENTS.md` first, then read one to three practice files
that match the current task.

## Entry Points

For Rust library or application work, start here:

- Rust maintainability, public API shape, modules, errors, dependencies, tests:
  `/Users/joshka/local/practice/src/content/guides/rust-maintainability.md`
- Documentation improvement workflow, pass depth, evidence, review narrative, docs drift:
  `/Users/joshka/local/practice/src/content/guides/documentation-workflow.md`
- Markdown, Rustdoc, examples, README/Rustdoc alignment, source links:
  `/Users/joshka/local/practice/src/content/guides/markdown-documentation.md`
- Rustdoc crate-quality checklist, including crate roots, public contracts, example coverage
  limits, experimental caveats, and doc validation:
  `/Users/joshka/local/practice/src/content/patterns/run-rustdoc-quality-pass.md`
- Source shape, reader locality, extraction, cohesion, coupling, naming, expression shape:
  `/Users/joshka/local/practice/src/content/guides/code-shape.md`
- Parsing, validation policy, state transitions, explicit inputs, side effects, async boundaries:
  `/Users/joshka/local/practice/src/content/guides/boundary-correctness.md`
- Structured errors, logging ownership, diagnostics, telemetry, failure visibility:
  `/Users/joshka/local/practice/src/content/guides/observability-and-failure.md`
- Change size, reviewability, validation scope, and review narrative:
  `/Users/joshka/local/practice/src/content/guides/software-change-preferences.md`
- Local jujutsu workflow, change descriptions, bookmarks, publication, recovery:
  `/Users/joshka/local/practice/src/content/guides/jj-workflow.md`

## Routing

When the user says they want to make project docs better, start with
`documentation-workflow.md`. If the project is a Rust crate and the work is about
Rustdoc/API docs, also read `run-rustdoc-quality-pass.md` and then
`markdown-documentation.md` only if writing style, examples, or Markdown mechanics matter.

When the task is a Rust code/API change, start with `rust-maintainability.md`.
Follow links from that guide to specific rule files only when the task touches that concern.

When the task is mostly about structure before behavior, read `code-shape.md`.
When it touches parsing, validation, state transitions, or side effects, read
`boundary-correctness.md`. When it touches errors, logging, diagnostics, or failure modes, read
`observability-and-failure.md`.

When the task spans review planning, change decomposition, or handoff quality, read
`software-change-preferences.md`. In jj repositories, also read `jj-workflow.md` before changing
local version-control state.

## Use Limits

Treat practice as durable preference guidance, not as proof of the target repo's current behavior.
Verify commands, APIs, docs, and tests in the live target checkout before editing or answering.

Prefer the target repo's local instructions when they conflict with practice. If the target repo has
no local answer, use practice to choose the default shape, validation level, and documentation
surface.

For docs changes in `/Users/joshka/local/practice` itself, follow that repo's `AGENTS.md` and keep
changes surgical: add or link one stable guidance unit rather than duplicating a whole guide.
