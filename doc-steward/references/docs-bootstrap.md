# Docs Bootstrap Guide

Use this for greenfield projects or repositories with little/no existing documentation.

## Bootstrap Objective

Create minimum viable docs that let a new contributor run, understand, and modify the project.

## Bootstrap Pack (Minimum)

1. README with project purpose and quick start.
1. Usage section with one end-to-end example.
1. Configuration section for required env vars/flags.
1. Basic troubleshooting for top 3 likely failures.
1. Initial architecture note (short, high-level boundaries and terms).

## Bootstrap Workflow

1. Derive truth from code, tests, and command output.
1. Write the shortest runnable path first.
1. Add expected output or success criteria.
1. Mark unknown assumptions explicitly.
1. Run lint and verify commands are copy/pasteable.
1. Run Unknown confirmation before finalizing docs.

## Unknown Budget (Bootstrap)

- Quick/touched-files pass: target 0-3 `Unknown` items.
- If above budget, resolve highest-impact unknowns first.
- End with: "Confirm whether to keep all remaining Unknowns, or list items to resolve now."

## Greenfield Checklist

- A new engineer can run the project using README steps.
- Terms are defined once and reused consistently.
- Setup assumptions are explicit (platform, toolchain, versions).
- Failure modes include concrete recovery steps.

## Pasteable Starter

```text
Use $doc-steward in Bootstrap mode.
Create a minimal README with quick start, usage, config, and troubleshooting.
Use only Confirmed claims from code/tests/CLI output; mark the rest Unknown.
```
