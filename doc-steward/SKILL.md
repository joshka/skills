---
name: doc-steward
description: >
  Write, rewrite, and review developer documentation (README.md, Rustdoc/docstrings, in-code
  comments, ADRs/design notes) with a focus on clarity, correctness, and drift prevention. Use
  when you want a docs plan, docs draft, docstring/comment pass, documentation audit, or a doc
  review rubric for proposed changes.
---

# Doc Steward

## Overview

Plan, write, and maintain high-signal documentation that stays correct as the code changes.

## Non-negotiables

- Treat docs (including docstrings/comments) as part of the public API.
- Prefer "why / constraints / invariants / edge cases" over restating code.
- Never invent rationale. If intent is not provable from code/tests/usages, ask one concise
  question or write a TODO stating what's unknown and what evidence is needed.
- Fix doc drift in the same diff as the behavior change.

## Workflow

1. Identify the doc artifact(s): README, API docs, guide, design note, or in-code docs.
2. Clarify audience and goal (ask up to 3 questions if missing).
3. Choose a doc type (see `references/diataxis.md`) and outline sections.
4. Gather evidence: call sites, tests, CLI help output, examples, public API, error paths.
5. Draft, then do a drift pass: verify identifiers, flags, invariants, and examples compile/run.
6. Review with `references/doc-review-rubric.md` and apply fixes.

## Modes

### Docs Plan

- Ask for missing context (audience, environment, success criteria).
- Produce: an outline, a "knowns/unknowns" list, and 3-7 concrete examples to include.

### Docs Draft

- Write directly against the outline.
- Include runnable examples where feasible; label pseudo-code.
- Keep Markdown lintable and consistent (see `references/markdown-style.md`).

### Docs Review

- Check for drift: names, flags, error cases, defaults, versions, compatibility.
- Prefer actionable edits (exact sentences/sections to change), not vague feedback.
- Call out any claims that are not supported by code/tests.

### Docstring / Comment Pass

- Apply drift prevention and "never invent rationale" (see
  `references/comment-steward-prompts.md`).
- Prioritize public APIs and high-complexity/high-fanout functions.
- If a docstring acts as a contract, update tests when you change that contract.

### Rustdoc Mode

- Follow Rustdoc conventions and section headers (see `references/rustdoc.md`).
- Prefer compiling doc tests and keeping examples minimal and focused.

## Resources

Load only what's needed for the task.

- `references/diataxis.md`: Pick the right doc type and structure.
- `references/doc-review-rubric.md`: Review checklist for docs quality and drift.
- `references/markdown-style.md`: Keep Markdown consistent and lintable.
- `references/comment-steward-prompts.md`: Prompts and templates for docstrings/comments.
- `references/rustdoc.md`: Rustdoc patterns (crate docs, item docs, sections, examples).
- `references/readme.md`: README structure and section templates.
