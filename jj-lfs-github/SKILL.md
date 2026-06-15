---
name: jj-lfs-github
description: Publish or prepare image, video, font, archive, model, generated media, or other binary assets for GitHub from a jj repository using Git LFS. Use when Codex is in a `.jj` repo and needs to add, update, PR, or push large or binary assets, especially screenshots/GIFs/site assets/release previews; when `git-lfs` or `.gitattributes` may be missing; or when jj reports LFS files as modified, stores raw blobs, exceeds snapshot size limits, or otherwise makes asset publication confusing.
---

# JJ LFS GitHub

## Core Rule

Do not assume jj will apply Git LFS clean filters when snapshotting or exporting a change. Verify the
actual Git object that will be pushed. If a binary asset must be LFS-backed, the committed blob in
Git must be a small pointer file, not the raw binary.

Use Git only for the narrow LFS handoff when needed. Keep normal source-of-truth work in jj unless
the verification below proves jj is about to publish raw binary blobs.

## Fast Workflow

1. Confirm the repo and asset situation.

   ```bash
   jj --no-pager status
   git lfs version
   git lfs track
   git check-attr -a -- path/to/asset.ext
   ```

1. If `git lfs version` fails or no LFS patterns apply, stop and ask before introducing LFS.
   Adding Git LFS changes clone, checkout, hosting quota, CI, and release behavior.

1. Add or narrow `.gitattributes` intentionally. Prefer path-specific tracking for generated site
   assets unless the repo already uses broad patterns.

   ```bash
   git lfs track path/to/asset.gif path/to/frame.png
   git check-attr -a -- path/to/asset.gif path/to/frame.png
   ```

1. Stage with Git and inspect the staged pointer before committing or pushing.

   ```bash
   git add .gitattributes path/to/asset.gif path/to/frame.png
   git diff --cached --no-textconv -- path/to/asset.gif path/to/frame.png
   git lfs status --porcelain
   ```

   The diff for each LFS asset must look like:

   ```text
   version https://git-lfs.github.com/spec/v1
   oid sha256:...
   size ...
   ```

1. If this is a simple one-change PR and jj has raw binary content in `@`, use a Git handoff branch
   from the intended base rather than `jj git push`.

   ```bash
   git switch -c owner/topic
   git add .gitattributes path/to/asset.gif path/to/frame.png other/touched/files
   git -c commit.gpgsign=false commit -m "Add asset-backed preview"
   git push -u origin owner/topic
   ```

   After committing, verify the committed blobs:

   ```bash
   commit=$(git rev-parse HEAD)
   git cat-file -s "${commit}:path/to/asset.gif"
   git cat-file -p "${commit}:path/to/asset.gif"
   git lfs ls-files
   ```

   The committed blob size should be around a pointer-file size, commonly near 130 bytes. The
   content must be the LFS pointer text, not GIF/PNG/JPEG bytes.

## Choosing The Publication Shape

Use the smallest workflow that preserves correctness.

- `jj-native`: Use normal `jj git push` only after verifying the exported Git commit contains LFS
  pointers for the assets.
- `git-handoff`: Use a temporary Git branch/commit from the intended base when jj snapshots or
  exported commits contain raw binary blobs but Git clean filters produce correct LFS pointers.
- `sideband/megamerge`: Consider a merge collector when LFS assets are generated, bulky, or
  frequently refreshed and should stay separate from a clean jj stack. Treat this as an explicit
  structure choice, not a default. Verify every exported head before pushing.

Megamerges can be useful when a review stack has normal code changes plus a generated-asset change
that needs different tooling. Keep the generated-asset parent separate so code review, rebases, and
LFS repairs do not rewrite every logical change in the stack.

## Failure Signals

If any of these appear, pause publication and inspect the real Git objects:

- `jj diff --stat` shows multi-megabyte binary additions for paths intended for LFS.
- `git cat-file -p <commit>:<asset>` starts with binary magic bytes such as `GIF89a` or PNG header
  bytes instead of `version https://git-lfs.github.com/spec/v1`.
- GitHub rejects the push for large files or shows full binary blobs in the PR diff.
- `git lfs ls-files` does not list the new assets after commit.
- `git status` is clean but `jj status` reports LFS files modified, or vice versa.

When `git status` and `jj status` disagree because checkout files are real LFS content while Git
commits store pointer blobs, trust verified Git objects for GitHub publication. Do not "fix" that
disagreement by rewriting unrelated jj history.

## PR Checklist

Before opening or updating a GitHub PR:

- `.gitattributes` is present and scoped intentionally.
- `git check-attr -a -- <asset>` reports `filter=lfs diff=lfs merge=lfs -text`.
- Staged or committed asset blobs are LFS pointers.
- `git lfs ls-files` lists each binary asset.
- LFS objects upload during push, or `git lfs push origin <branch>` succeeds.
- The PR body mentions LFS-backed assets when relevant.

Use `gh pr create` or `gh pr merge --auto` only after these checks pass.
