# Documentation Review Rubric

Use this checklist to review existing docs or a doc diff. Prefer concrete edits over vague
feedback.

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
