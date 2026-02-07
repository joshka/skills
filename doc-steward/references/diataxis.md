# Diátaxis Quick Guide

Use this to choose the right kind of documentation and avoid mixing goals.

## Pick A Doc Type

### Tutorial (learning-oriented)

Teach a beginner by doing.

- Focus: learning and confidence.
- Structure: prerequisites → steps → explanation only as needed → recap → next steps.
- Include: runnable "happy path" examples.
- Avoid: exhaustive options, edge-case catalogs, deep theory.

### How-to (goal-oriented)

Help someone accomplish a task they already understand.

- Focus: a specific outcome.
- Structure: goal → prerequisites → steps → verification → troubleshooting.
- Include: copy/paste snippets, expected output, common failures.
- Avoid: long introductions, background explanations.

### Reference (information-oriented)

Describe the system precisely.

- Focus: correctness and completeness.
- Structure: concepts/glossary → API surface → options → defaults → errors.
- Include: exact names, types, constraints, version/feature notes.
- Avoid: narrative and step-by-step flows (link to how-tos instead).

### Explanation (understanding-oriented)

Build intuition and rationale.

- Focus: why the system is the way it is.
- Structure: problem → constraints → tradeoffs → design → implications.
- Include: diagrams, invariants, "why not X", pitfalls.
- Avoid: step-by-step instructions (link to tutorials/how-tos instead).

## Common Failure Modes

- Writing a tutorial but adding reference tables mid-stream.
- Writing a reference doc but hiding key defaults in prose.
- Writing a how-to that starts with a history lesson.
- Writing an explanation that never states the practical consequences.
