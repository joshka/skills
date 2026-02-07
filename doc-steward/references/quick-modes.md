# Quick Modes

Use these short invocations when you want low-touch setup.

## Profile Dimensions

- `role`: `contributor` | `owner`
- `reach`: `touched-files` | `repo-wide`
- `depth`: `quick` | `full`

## One-Line Invocations

```text
Use $doc-steward in Bootstrap mode as a contributor, focused on touched files, with a quick pass.
```

```text
Use $doc-steward in PR Narration mode as a contributor, focused on touched files, with a quick pass.
```

```text
Use $doc-steward in Ratchet mode as a repo owner, with repo-wide scope and full detail.
```

```text
Use $doc-steward in Recovery mode as a repo owner, with repo-wide scope and full detail.
```

```text
Use $doc-steward in Bulk Update mode as a repo owner, with repo-wide scope and full detail.
```

```text
Use $doc-steward in Process Improvement mode as a repo owner, repo-wide, with a quick pass.
```

```text
Use $doc-steward with the Greenfield Full Coverage approach as a repo owner, repo-wide, with full detail.
```

```text
Use $doc-steward in Docstring / Rustdoc mode as a repo owner, repo-wide, full detail, deep docs.
```

## Reuse Line (When Profile Is Not Persisted)

At the end of a run, output this compact line for next time:

```text
Next run: Use $doc-steward in <mode>, as a <role>, with <reach> scope and <depth> detail.
```

## Unknown Confirmation

At the end of a pass, ask for explicit confirmation to keep remaining unknowns as-is.
