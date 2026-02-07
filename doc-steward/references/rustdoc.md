# Rustdoc Patterns

Use Rustdoc to document behavior, constraints, and contracts. Prefer examples that compile.

## Crate And Module Docs (`//!`)

Include, in roughly this order:

- One-sentence summary: what the crate/module is for.
- Quick start example (doc test) showing the main happy path.
- Key concepts and terminology used by the API.
- Feature flags and what they enable (and what the defaults are).
- Important constraints/assumptions (terminal size, color support, threading, I/O).
- Non-goals (what the crate intentionally does not do).

## Item Docs (`///`)

Keep the first line as a brief summary. Add sections only when relevant.

- `# Examples`: small, focused, doc-test friendly.
- `# Panics`: document panic conditions (or state "Does not panic" if important).
- `# Errors`: document error variants/conditions and recovery guidance.
- `# Safety`: for `unsafe` functions/traits/impls, state the caller obligations precisely.
- `# Performance`: complexity notes, allocation behavior, caching, hot paths.

## Document Contracts

- Prefer documenting invariants and edge cases to restating the implementation.
- Treat existing docs as a contract unless proven wrong by tests or call sites.
- If you change the contract, update tests and docs in the same change.

## Doc Tests

- Prefer runnable examples.
- Use `no_run` when running would require external systems, but compilation is valuable.
- Use `ignore` sparingly; it hides breakage.
