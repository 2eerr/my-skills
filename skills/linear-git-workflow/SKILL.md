---
name: linear-git-workflow
description: "Linear-history git workflow — never commit to the default branch; use feature branches named `<type>/<kebab-slug>` (Conventional-Commits); write imperative commits; integrate via rebase + `git merge --ff-only` (no merge commits); push only the default branch. Covers multi-PC sync. Use when committing, pushing, or merging in any repo with protected-branch linear history."
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
   optionally prefix an issue number (`feat/123-…`). Some repos extend the set (`hotfix/`,
   `release/`, `design/`, `spike/`) — follow the repo's own list when it defines one.
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

## Procedure — SYNC (multi-PC / shared remote)

GitHub is the single source of truth when several machines work the same repo:

- **Pull at the start of every session** — before reading, writing, or changing anything:
  `git pull {remote} <branch>` or `git fetch {remote}` + `git merge --ff-only {remote}/<branch>`.
  Always start new branches from the latest integration branch. Pulling is safe and expected —
  never skip it.
- **Conflicts: report, never auto-resolve.** List the conflicted files, ask how to resolve
  (theirs / ours / manual), and let the user decide.
- **Never run destructive git automatically** — no commit, push, merge, rebase, `reset --hard`,
  or branch deletion unless the user explicitly asks (or triggers a defined shortcut).
- **Remind after every commit** (don't push): "push `<branch>` when ready — keeps other PCs
  in sync."
- **Pinned remote URL avoids account pickers:** embed the username in the HTTPS remote
  (`https://{owner}@github.com/{owner}/{repo}.git`) so the credential manager selects the
  right account; it is a per-clone local setting and the username is public, not a secret.

## Variant — two permanent branches (`{default}` + `{dev}`)

Repos with an integration branch run the same discipline twice:

- `{default}` = production, `{dev}` = integration/testing; **both are permanent, never deleted,
  and the only branches ever pushed.** All work happens on topic branches cut from `{dev}`.
- Flow: topic branch → rebase + `--ff-only` into `{dev}` → (only when the user approves)
  `{dev}` → `--ff-only` into `{default}` → push both.
- **`{dev}` is always at or ahead of `{default}`** — `{default}` must never hold a commit
  `{dev}` lacks. The graph stays one straight line across both.

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
