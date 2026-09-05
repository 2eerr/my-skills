---
name: linear-git-workflow
description: "Commit and publish work with a strict linear-history git workflow — never commit directly to the default branch; work on feature branches named `<type>/<kebab-slug>` (Conventional-Commits types); write short imperative commits that describe the actual change; integrate by rebasing each branch onto the default branch and fast-forwarding it with `git merge --ff-only` (no merge commits); then push the default branch. Use when committing, pushing, pulling, or merging branches in any repo that keeps protected-branch linear history (including repos whose AGENTS.md defines git!/push!-style shortcuts)."
metadata:
  internal: true
---

# Linear Git Workflow (feature branches, no merge commits)

The git discipline for committing and publishing work in a repo with a **protected default
branch** and a **linear history** requirement. Applies to any repo/agent — read the project's
own instructions (e.g. its `AGENTS.md`) for repo-specific values first.

## Project config (read per repo, never hardcode)

| Variable | Meaning | Typical |
|---|---|---|
| `{default}` | protected branch, never committed to directly | `main` |
| `{remote}` | publish remote | `origin` |
| `{owner}` | GitHub account/org the remote is pinned to | e.g. `https://{owner}@github.com/{owner}/{repo}.git` so pushes never prompt |
| `{author}` | repo-local identity | `user.name` + `user.email` set per repo |
| trigger phrases | shortcuts the repo's `AGENTS.md` may bind (e.g. a "commit" or "publish" phrase) | follow them as written |

## Hard rules

1. **Never commit directly to `{default}`.** Any change goes on a feature branch first.
2. **Branch naming:** `<type>/<kebab-slug>` where `<type>` is a Conventional-Commits type —
   `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `perf`, `ci`, `style`, `build`. Keep the
   slug short and descriptive (e.g. `feat/add-data-pack-skill`, `fix/skill-frontmatter-colon`);
   optionally prefix an issue number (`feat/123-…`).
3. **Linear history, always:** the graph stays a single straight line — **no merge commits.**
   Integrate with rebase + `git merge --ff-only`; pull with `git pull --rebase`; never use
   `--no-ff`. Remote PRs land as squash- or rebase-merges.
4. **Commit messages:** short imperative subject that **describes the actual change** (never a
   generic "update"); one line per logical change; body optional for multi-part commits.
5. **Only `{default}` is ever pushed to `{remote}`.** Feature branches stay local — they exist
   only to be rebased onto `{default}` and fast-forwarded into it; nothing else goes to the
   remote.
6. **Never** commit secrets; inspect `git status` + `git diff` before staging.

## Procedure — COMMIT (on a feature branch)

1. If currently on `{default}`, create the branch first: `git checkout -b <type>/<slug>` named
   for the actual change.
2. Inspect: `git status` and `git diff` — know exactly what is landing.
3. Stage and commit: `git add -A` then a descriptive imperative message.
4. If the commit fails (hooks reject), fix the cause and make a **new** commit — do not amend
   or bypass hooks.

## Procedure — PUBLISH (commit → integrate → push `{default}`)

Run in this exact order, keeping history linear:

1. **Commit pending work** (COMMIT procedure above) on its feature branch.
2. **For every other local branch except `{default}`** — rebase it onto `{default}`, then
   fast-forward `{default}` onto it:
   `git rebase {default} <branch>` → `git checkout {default}` → `git merge --ff-only <branch>`.
   Process branches oldest-first; a branch already contained in `{default}` is a no-op.
   **No merge commits** — if `--ff-only` refuses, rebase again; never fall back to a merging
   merge.
3. **Push `{default}`** to `{remote}`: `git push {remote} {default}`.
4. **Clean up:** delete only the local branches that merged successfully into `{default}` —
   `git branch --merged {default}` (excluding `{default}` itself); **never** delete unmerged
   branches.

## Commands

```bash
git checkout -b <type>/<slug>                       # start work (never on {default})
git status; git diff                                # inspect before staging
git add -A && git commit -m "<imperative subject>"  # commit
git pull --rebase                                   # stay linear when syncing
git rebase {default} <branch>                       # integrate step 2a
git checkout {default} && git merge --ff-only <branch>   # integrate step 2b
git push {remote} {default}                         # publish
git log --oneline --graph --all                     # verify the graph is one straight line
```

## Checklist (done when)

- [ ] No commit was ever made directly on `{default}`.
- [ ] Every branch follows `<type>/<kebab-slug>`.
- [ ] `git log --graph` shows a single straight line — zero merge commits.
- [ ] Each commit subject is imperative and describes the actual change.
- [ ] Only `{default}` was pushed — no feature branch exists on `{remote}`.
- [ ] `{default}` on `{remote}` matches local `{default}` after publishing.
- [ ] Only fully-merged local branches were deleted after publishing; unmerged ones kept.

## Notes & gotchas

- **Never** `git push --force` `{default}`. Because feature branches are never pushed, rebasing
  them needs no force-push at all; if a stale feature branch exists on `{remote}`, delete it
  (`git push {remote} --delete <branch>`) instead of updating it.
- Do not amend or drop commits that are already pushed and shared.
- Keep repo docs (README/CHANGELOG) in sync with the change in the same branch — the commit
  should land the code/content *and* its documentation.
- Windows note: `LF will be replaced by CRLF` warnings on commit are informational (autocrlf);
  they do not block the commit.
- If `{default}` moved while you worked, re-run the rebase before the `--ff-only` merge — the
  fast-forward must always succeed cleanly.
