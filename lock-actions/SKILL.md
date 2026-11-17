---
name: lock-actions
description: >
  Move a repo from SHA-pinned GitHub Actions to a workflow lockfile
  (.github/workflows/actions.lock via gh actions-lock), add the verify job to CI, and open a PR.
  Use when asked to "lock actions", onboard a repo to actions.lock, fix a failing lockfile job,
  or whenever a task involves actions.lock, gh actions-lock, or replacing SHA-pinned actions.
---

# lock-actions

## What this is for

CI runs third-party code with access to the repo's token and, in release workflows, publish
credentials. A tag such as `v4` is a pointer that its owner, or an attacker who takes over the
owner's account, can move to malicious code at any time. The goal is that **every workflow run
executes exactly the action code someone reviewed, and any change to that code shows up as a diff
in a PR.**

SHA pins in workflow files already get most of the way there, but they leave three gaps:

- **Transitive actions.** A pinned action can call other actions by a mutable tag. The lockfile
  records those too, and the runner enforces them.
- **Impostor commits.** Forks share git objects with upstream, so a SHA from a fork resolves under
  the upstream name. The tool refuses commits that aren't reachable from the upstream ref.
- **Readability.** `@<sha> # v2` drifts: the comment can stop matching the SHA. With a lockfile,
  workflows show a real ref and the lockfile records the commit, and the runner checks one against
  the other.

What it does **not** protect against: the reusable-workflow call itself (the actions *inside* a
reusable workflow are covered by its source repo's lockfile, but the caller must SHA-pin the
call), container images,
code an action downloads at run time (`npx`, `curl | sh`), and the runner image. Keep those in mind
instead of treating a locked repo as fully immutable.

Each step below says why it exists. If a repo doesn't fit a step, decide based on that reason
instead of forcing the mechanism.

## Ground rules

- One repo per run, one PR per repo. Never push to main. A person reviews every lockfile change,
  because review is the whole point.
- Never enable repo or org policies (such as "Require lockfile"). The maintainer does that after merge.
- Never run `--accept-moved`, never delete the lockfile, and never edit it by hand. Each of these
  silently accepts whatever upstream points at today, which undoes the protection.

- **Onboarding must not change what runs.** Every locked commit should equal the SHA the workflow
  pinned before. Any difference is a dependency upgrade, and it needs its own review.

Written against: `TOOL_VERSION=v0.4.1`, `zizmor 1.32.0`, and the upstream skill as of
2026-11-13. The lockfile must be generated with the same version CI verifies with, so check
`gh actions-lock --version` and stop on a mismatch (`gh extension install
github/gh-actions-lock --pin v0.4.1 --force`, or ask). Also check `gh release list -R
github/gh-actions-lock -L 1`. If there's a newer release, read its notes and the upstream
`skills/gh-actions-lock/SKILL.md` diff, tell the maintainer what changed, and propose edits to
this skill.
Don't bump versions silently.

## Version control

Use jj when the repo has `.jj/` (usually colocated with git), otherwise git. If both exist, prefer
jj: mixing git commands into a jj repo leaves jj to reconcile them, which can be confusing.
`gh actions-lock` only reads and writes the working copy, so it works the same with either.

Don't assume the default branch is `main`. In jj, `trunk()` resolves to the remote's default
branch. In git, use `origin/HEAD` (run `git remote set-head origin -a` if it isn't set).

| Operation | jj | git |
| --- | --- | --- |
| Fetch | `jj git fetch` | `git fetch origin` |
| Start a change on latest default branch | `jj new 'trunk()'` (leaves existing work alone) | `git status --porcelain` must be empty (otherwise use `git worktree add`), then `git switch -c ci/lock-actions origin/HEAD` |
| Show the workflow diff | `jj diff .github/workflows` | `git diff .github/workflows` |
| Describe or commit | `jj describe -m 'ci: lock GitHub Actions with actions.lock'` | `git add .github/workflows && git commit -m 'ci: lock GitHub Actions with actions.lock'` |
| Push a PR branch | `jj git push -c @` (prints the branch name) | `git push -u origin ci/lock-actions` |
| Restore the lockfile from a known-good revision | `jj restore --from 'trunk()' .github/workflows/actions.lock` | `git checkout origin/HEAD -- .github/workflows/actions.lock` |

Open the PR with `gh pr create --head <branch> --body-file <file>`.

## Survey first

Why: the steps below assume the repo already follows the CI conventions in this skill. Many repos
were set up at different times and drift. Finding out first lets you adapt the steps or recommend
fixes, instead of forcing the script onto a repo it doesn't fit.

Read `.github/` and the repo's rules (read-only), then report a table: convention, status
(ok / differs / missing), evidence (file:line or API output), and recommendation.

| Convention | Check |
| --- | --- |
| One main check workflow | `check.yml` or `ci.yml` exists and holds most jobs |
| `required` aggregator | A `required` job with `if: ${{ always() }}` that fails on `failure`, `cancelled` or `skipped` needs |
| Everything gates | Every non-release job in that workflow is in `required.needs`. List any that aren't (e.g. tui-widgets' `zizmor`) |
| Only `required` is required | `gh api repos/{owner}/{repo}/rules/branches/<default-branch>` plus classic protection: the required check list is just `required`. Other entries are stale names or separate gates |
| Triggers | `pull_request`, `push` to the default branch, and `merge_group` if a merge queue is on. No path filters on the main check workflow |
| Least privilege | Top-level `permissions:` is `{}` or `contents: read`. Each checkout sets `persist-credentials: false` |
| Pins | Every remote `uses:` is pinned to a 40-char SHA with a `# ref` comment (or already locked). The comment matches the action's recommended ref (see the ref policy in step 1) |
| Dependabot | A `github-actions` ecosystem entry with a `cooldown` |
| Workflow linting | zizmor and actionlint jobs exist. Container images are pinned by digest |
| Shape | Published action manifests, reusable workflow calls and local `./` actions (these feed into preflight) |

Then decide, and say which you chose:

- **Fits:** continue with preflight.
- **Small gaps** (a job missing from `needs`, a stale ref comment, a missing `persist-credentials`):
  fix them in the same PR, each listed under "Other changes" with its reason.
- **Big gaps** (no `required` job, required checks pointing elsewhere, no main check workflow, no
  Dependabot): stop. Recommend the change, and offer to do it as its own PR before locking. Don't
  quietly restructure CI inside a lockfile PR.
- **A convention here looks wrong for this repo:** say so, and suggest a change to this skill
  rather than bending the repo to fit.

## 0. Preflight

Why: some repo shapes need a decision before the tool runs, because it will happily make the repo
less safe for other people or leave gaps that look covered.

```sh
gh actions-lock --version                                     # must be v0.4.1
find . \( -name action.yml -o -name action.yaml \) -not -path './.git/*' -not -path './.jj/*' -not -path './node_modules/*'
grep -rnE '^\s+uses:\s+[^.$ ]+/.+\.ya?ml@' .github/workflows   # job-level reusable workflow calls
grep -rnE 'uses:\s+\./' .github/workflows                       # local actions
grep -rnE 'uses:\s+docker://|image:' .github/workflows          # container refs
# record the "before" pins, for the commit check in step 1
grep -rhoE 'uses:\s+[^ ]+@[0-9a-f]{40}' .github/workflows | sort -u > "${BEFORE_PINS:=$(mktemp)}"   # keep $BEFORE_PINS for step 1
```

- **Any action manifest that other repos may consume** (`action.yml` or `action.yaml`, at the root
  or in a subdirectory, especially `using: composite` with remote `uses:`): STOP and ask. Its pins
  and `uses:` refs must stay exactly as they are. A rewrite strips the SHA pins that callers
  without lockfiles rely on. Scoping the run to workflow paths isn't enough on its own:
  `--migrate-local-actions` rewrites action files too. Afterwards, confirm that the diff doesn't
  touch any published manifest.
- **Reusable workflow calls** (`jobs.<id>.uses: owner/repo/...yml@ref`): the lockfile doesn't
  cover the call. Keep it SHA-pinned with a `# vX.Y.Z` comment and list it in the PR.
- **Local `./` actions that call remote actions**: a `./` path runs whatever is in the workspace,
  so it can't be locked. Run with `--migrate-local-actions` (rewrites to `$/`, which runs at the
  workflow's own commit) and review every rewrite. If any rewrite touches a published manifest,
  stop.
- Fetch, then start a new change on the latest default branch (see the table).

## 1. Lock

Why: this records the exact commit behind every direct and transitive action, so later changes
show up as reviewable diffs.

```sh
gh actions-lock
jj diff .github/workflows        # or: git diff .github/workflows
```

**Check two things for every `uses:` line.**

1. **The commit didn't change.** Compare each locked `commit:` in `actions.lock` against
   `$BEFORE_PINS`. A mismatch is an upgrade, not onboarding.
2. **The ref is the one you want.** The ref in the workflow decides what Dependabot updates and
   what a reader believes is running. The tool picks *a* ref that contains the pinned commit,
   ignoring your `# comment`. A branch is only picked when its head happens to equal the pinned
   commit, so the result can differ from one day to the next. Seen on tui-widgets:
   - `dtolnay/rust-toolchain@<sha> # master` became `@v1`.
   - On one run, `Swatinem/rust-cache@<sha> # v2` became `@master`.

**Changing the ref resolves it fresh. It doesn't keep the old commit.** In the review,
switching rust-toolchain from `@v1` back to `@master` locked `df2a88e` instead of `683b588`. So,
for each ref you want to change:

- If the desired ref points at the same commit as before (an exact tag usually does, e.g. check
  with `git ls-remote --tags <repo-url>`), change it and rerun `gh actions-lock`.
- If it points somewhere else, choose one of these and say which in the PR: accept the move as a
  reviewed upgrade (read the upstream diff between the two commits), keep the ref the tool chose
  for now, or keep the SHA pin.

Ref policy, decided once per action:

- An exact `vX.Y.Z` tag when one points at the commit you want. Each update is then a named
  version that Dependabot can propose.
- The README-documented branch plus inputs for actions designed around branches
  (dtolnay/rust-toolchain: `@master` + `toolchain:`). Branch refs never move on their own: they
  advance only with `--relock`, so they need someone to refresh them on a schedule.
- Never a major-only tag (`@v2`), and never a default branch the action doesn't document.

**The existing pins aren't necessarily right either.** A repo's current `@<sha> # master` or
`# v2` may have been copied from somewhere, or reflect a choice nobody revisited. Check each action
against this policy. If its README recommends a different ref (a channel branch, a newer major,
or a tag rather than a branch), changing it is fine when you have a reason. Every such change
goes in the PR body under "Ref changes" with:

- old ref → new ref
- old commit → new commit (or "same commit")
- why the new ref is the right one, with a link to the action's README or release notes

A ref change with no stated reason doesn't get merged.

After any hand edit, run `gh actions-lock` again and repeat the commit check.

## 2. CI job

Why: a stale or tampered lockfile should fail a required check before merge, not at run time
after other jobs have already started.

Put it in the repo's main check workflow, `check.yml` or `ci.yml`, whichever holds the
`required` job. Don't create a new workflow file.

**How required checks work in these repos:** each repo's main check workflow ends with one
aggregate job called `required`. It has `needs:` on every job that must pass and
`if: ${{ always() }}`, and it fails if any of those jobs failed, was cancelled, or was skipped.
Branch protection or rulesets mark only `required` as a required check. To make a job gate merges,
add it to `required`'s `needs:`. Nothing changes in the repo settings, and renaming or adding jobs
never leaves a PR waiting on a check that no longer exists.

So:

1. Add the `lockfile` job below to the main check workflow.
2. Add `lockfile` to `required.needs`.
3. Confirm once that `required` really is the required check (`gh api
   repos/{owner}/{repo}/rules/branches/<default-branch>`, or the classic branch protection). Also
   confirm that the workflow runs on every PR and push to the default branch, plus `merge_group:`
   if a merge queue is on. Don't add path filters.
4. If the repo has no `required` job yet, stop and ask. Adding one, and switching branch
   protection over to it, is its own change.

This required check and GitHub's "Require lockfile" policy are separate controls.

```yaml
  lockfile:
    name: actions lockfile
    runs-on: ubuntu-slim
    permissions:
      contents: read
    env:
      GH_TOKEN: ${{ github.token }}
    steps:
      - uses: actions/checkout@vX.Y.Z   # same tag as the rest of the repo
        with:
          persist-credentials: false
      - run: gh extension install github/gh-actions-lock --pin v0.4.1
      - run: gh actions-lock --verify
```

Then run `gh actions-lock` again, because the new job adds a `uses:`.

Don't add the "update job" the GitHub docs suggest (CI regenerates and pushes the lockfile). It
needs `contents: write` and the `workflow` scope, and it writes to the repo automatically. A plain
regeneration keeps existing locked commits. But new refs resolve to whatever they point at that
moment, so the job would bring in new code that nobody reviewed before it lands.

## 3. Related pieces

Why: these either break once the repo is locked, or are the remaining unpinned code in CI.

- **zizmor**: before 1.32.0, it flags lockfile-pinned tags as `unpinned-uses`.
  `zizmorcore/zizmor-action` v0.8.0 runs the latest zizmor by default, so only a pinned
  `version:` input older than 1.32.0 needs bumping.
- **Container images** (`docker://rhysd/actionlint:1.7.12`, `container:`, `services:`): the
  lockfile doesn't cover them. Pin by `@sha256:` digest, with the tag in a comment.
- **Dependabot**: keep the `github-actions` ecosystem and `cooldown`. Dependabot updates
  `actions.lock` in its own PRs, and the tool reads the cooldown for its `fresh-tag` notice.
- `runs-on: ubuntu-latest` is out of scope.

## 3b. Maintainer docs

Why: after this PR, people (and agents) will add actions, merge Dependabot bumps, and hit lockfile
failures without having read this skill. The repo needs its own short explanation.

- Copy `assets/github-actions.md` to `docs/github-actions.md`. Fill in `{{CHECK_WORKFLOW}}`
  and `{{TOOL_VERSION}}`, and delete anything that doesn't apply to this repo, such as the
  branch-ref section if the repo has no branch refs. If the repo already has a CI or contributing
  doc, add a link from it, or merge the content in, rather than keeping two docs.
- If the repo has `AGENTS.md`, add one bullet to its CI or workflow section:
  `- GitHub Actions are locked in .github/workflows/actions.lock. After changing any uses:, run
  gh actions-lock and commit both files. See docs/github-actions.md. Never edit or delete the
  lockfile by hand.`
  Don't create an `AGENTS.md` if the repo doesn't have one.
- Keep it public-safe: no personal details, no references to this skill.

When the tool pin in CI changes later, update the version in `docs/github-actions.md` in the same
PR.

## 4. Verify and PR

Why: the PR is where a person reviews what got pinned. Its body should make that review quick.

```sh
gh actions-lock --verify          # must exit 0
actionlint                        # if installed
```

Describe or commit, push, and open a PR (see the version-control table). The PR body lists:

- the tool version
- the survey table, or a one-line "fits all conventions"
- "Other changes": small survey fixes made in this PR, each with its reason
- "Ref changes": every ref that differs from the original pin's comment, whether the tool picked
  it or you did, with old → new ref, old → new commit, and the reason (see step 1)
- what's still not covered (reusable workflows, images, `./` actions)
- the transitive dependencies the tool reported

## Fixing a failing lockfile job later

Why: a failure means either routine drift (fix it) or upstream code that changed under a
reviewed ref (a person decides). The categories say which.

Run `gh actions-lock --verify --json 2>/dev/null` and act on each finding's `remediation`:

Only `error` findings fail the job. Warnings such as `ref-moved` are information, not a reason to
change anything.

- `not-pinned`, `stale`, `ref-changed`, `repo-renamed`: run `gh actions-lock` and commit.
- Lockfile corrupted by an accidental edit or a bad merge: restore it from a known-good revision
  rather than relocking.
- `tag-commit-mismatch`, `incomplete-lockfile`: look at the upstream diff between the two commits,
  then `--relock`, scoped to the affected workflow paths. `--relock` re-resolves *every* ref in
  scope, including unrelated floating branches. Afterwards, review every commit that changed, not
  just the one you were fixing.
- `unreachable-pin`, `misleading-sha`, `repo-replaced`: STOP and report to the maintainer. It's either
  rewritten history or an attack, and the tool can't tell which.
- `reachability-unknown`, `ancestry-unknown`: GitHub didn't answer. Rerun the job and change
  nothing.

To undo a lockfile change, restore it from a known-good revision (see the version-control table)
and run `gh actions-lock` again.
