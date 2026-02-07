# Tone And Style Lock

Use this when improving existing docs without changing the project's established voice.

## Objective

Increase clarity and correctness while preserving local style conventions.

## Style-Lock Workflow

1. Sample representative docs before editing.
1. Capture voice signals: formality, sentence length, terminology, structure.
1. Preserve existing naming and conceptual vocabulary.
1. Normalize only when inconsistency causes confusion.

## What To Preserve

- Domain terms and canonical phrasing.
- Heading patterns and section ordering where already coherent.
- Example depth appropriate to target audience.

## What To Improve

- Ambiguous language and contradictory statements.
- Missing prerequisites and success criteria.
- Drift between docs and runtime behavior.

## Quick Style Snapshot Template

```text
Tone: <formal|neutral|conversational>
Audience: <maintainer|contributor|operator|user>
Preferred structure: <short paragraphs/lists/tables>
Terms to keep: <list>
Terms to avoid: <list>
```
