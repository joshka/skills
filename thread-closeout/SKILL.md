---
name: thread-closeout
description: Produce a reviewable end-of-thread closeout for Codex work. Use when the user asks to close out, wrap up, summarize, retrospect, hand off, audit, or extract lessons from a Codex thread/session, especially when they want what happened, current repo/branch/PR state, validation, failures, follow-ups, memory candidates, or improvement routing for the project, Codex, tooling, skills, or process.
---

# Thread Closeout

## Operating Stance

Create a closeout artifact, not a diary and not an automatic cleanup action.

Prefer the transcript as the primary evidence. The session usually contains enough information to
derive files touched, commands run, failures, PRs/issues discussed, and follow-up state. Inspect the
repo, version-control state, GitHub, or local session files only to verify current state, fill
missing identifiers, or avoid false claims. If the current thread id is needed, read
`CODEX_THREAD_ID`. Codex sessions are stored under `~/.codex/sessions`.

Do not mutate files, create issues, update `AGENTS.md`, write memory, post comments, commit, push,
or change branches/bookmarks unless the user explicitly asks for that action. Separate reviewable
candidates from applied changes.

## Workflow

1. Identify the closeout scope.
   - Default to the current thread when the user says "this thread" or "close out".
   - If the user names another thread, session, rollout, PR, or repo, use that as the scope.
   - If the scope is ambiguous and cannot be inferred safely, ask one concise question.
1. Collect transcript evidence.
   - Reconstruct the request, important turns, decisions, tool results, failures, recoveries, and
     final answer.
   - Preserve explicit user corrections and instructions.
   - Treat missing final context as a signal to verify, not as permission to invent.
   - Prefer information already available from the session over proposing or running extra
     collectors.
1. Verify current state when useful.
   - In a repo, inspect working tree state, current branch/bookmark/change, recent commits, and
     changed files.
   - In a `jj` repo, prefer `jj --no-pager status`, `jj --no-pager log`, and `jj --no-pager desc`
     over raw Git workflow commands.
   - Check PRs, issues, CI, or reviews only when the transcript suggests they matter or the user
     asks for them.
1. Distinguish evidence quality.
   - Label facts supported by transcript or live state as facts.
   - Label plausible but unverified conclusions as inferences.
   - Mark unknowns explicitly instead of filling gaps.
1. Route improvements.
   - Convert friction into actionable candidates only when there is enough evidence.
   - Avoid recording trivial one-off mistakes, purely local noise, or issues already resolved with
     no durable lesson.
   - Keep project improvements, Codex/tooling improvements, memory candidates, and skill updates
     separate.
1. Produce the closeout.
   - Keep it concise by default. Expand only when the thread was long, risky, or explicitly
     retrospective.
   - Use markdown tables when they make follow-ups or improvement routing easier to scan.
   - Link the thread as `codex://threads/<thread-id>` when a thread id is available.
   - Link issues and PRs with descriptive markdown titles, not bare references. Use titles such as
     `[openai/codex#12345: Short issue title](https://github.com/openai/codex/issues/12345)` when
     the title is known.

## Closeout Template

Use this structure as a default. Omit empty sections when they add no signal, but keep failures,
validation, and follow-ups when any relevant evidence exists.

```markdown
# Thread Closeout

## Identity And State

- Thread: `codex://threads/<thread-id>`
- Workspace:
- Branch / bookmark / change:
- PR / issue links: titled markdown links when known
- Final status: completed | partially completed | parked | blocked | abandoned
- Working tree state:
- CI / review / merge state:

## Outcome

What was requested, what was achieved, what changed during the thread, and what remains unresolved.

## Work Product

- Files changed:
- Artifacts created:
- Commands run:
- Tests/checks run:
- PRs/issues/comments/docs updated:
- Decisions made:

## Validation

- Passed:
- Failed:
- Not run:
- Manual verification:
- Assumptions still unverified:
- Validation debt:

## Failures, Near-Misses, And Recovery

For each useful item:

- Symptom:
- Root cause:
- Recovery:
- Still relevant:
- Preventable next time:
- Evidence:

## Friction Log

- Model behavior:
- Process/workflow:
- Tooling/Codex/app-server/GitHub/jj:
- Codebase structure:
- Test/build/release setup:
- Library/API/dependency:
- Environment/auth/permissions:
- User/agent handoff ambiguity:

## Effectiveness Snapshot

- Goal completion: complete | partial | blocked | abandoned
- Verification quality: strong | adequate | weak | none
- Scope control: tight | acceptable | drifted
- Rework level: low | moderate | high
- User steering needed: none | useful | avoidable
- Confidence in final state: high | medium | low

## Lessons Learned

- Repo facts:
- Product/domain facts:
- Debugging techniques:
- Process lessons:
- Agent-behavior lessons:

## Improvement Routing

| Finding | Evidence | Category | Destination | Priority | Suggested action |
| --- | --- | --- | --- | --- | --- |

Destinations can include current PR, new issue, `AGENTS.md`, project docs, skill update,
Codex/tooling feedback, memory candidate, or no action.

## Follow-Ups

| Item | Owner | Context | Safe to defer? | Suggested next step |
| --- | --- | --- | --- | --- |

## Memory / Skill Candidates

Reviewable candidates only. Do not write automatically unless explicitly requested.

- Candidate memory:
- Candidate `AGENTS.md` rule:
- Candidate skill improvement:
- Candidate project convention:
- Rejected as too noisy:
```

## Failure Taxonomy

Use this taxonomy to avoid vague retro language:

- **Model**: wrong assumption, missed instruction, hallucinated state, overreach, poor
  prioritization, weak final summary.
- **Process**: unclear goal, missing checkpoint, wrong workflow, late scope discovery, bad handoff.
- **Tooling**: Codex, app-server, browser, GitHub, `jj`, MCP, shell, flaky command, missing API,
  confusing error.
- **Codebase**: confusing structure, missing test surface, weak docs, surprising API, brittle
  coupling.
- **Library/dependency**: version mismatch, undocumented behavior, semver surprise, platform quirk.
- **Environment**: auth, permissions, network, local setup, OS/platform, path or config issue.
- **Validation**: test gap, unchecked assumption, manual verification gap, CI not observed.

## Output Rules

- Lead with the closeout artifact, not a long explanation of how it was produced.
- Mention the evidence source when it matters: transcript, live repo state, GitHub, session file, or
  inference.
- Be candid about failures and avoid overstating certainty.
- Keep improvement recommendations burden-aware and easy to accept, reject, or defer.
- Do not include raw private transcript excerpts unless the user asks for them.
- Do not recommend helper scripts or automation as a default follow-up. Prefer the session-derived
  closeout first, and suggest automation only after repeated closeout friction is evident.
