---
name: git-workflow
description: "Industry-standard git workflow for speed and multi-PC collaboration — GitHub Flow with merge commits, feature branches, no rebase conflicts, and fast Windows performance. Use when committing, pushing, merging, or syncing across multiple machines. Covers both single-branch and dev+main patterns."
metadata:
  internal: true
---

# Git Workflow (industry-standard, multi-PC optimized)

The fastest, most reliable git discipline for teams working across multiple PCs. Based on
**GitHub Flow** — the industry standard used by most product teams. Optimized for Windows
repos with tens of thousands of files.

## Project config (read per repo, never hardcode)

| Variable | Meaning | Typical |
|---|---|---|
| `{default}` | primary branch (protected) | `main` |
| `{dev}` | integration branch (optional) | `dev` |
| `{remote}` | publish remote | `origin` |
| `{owner}` | GitHub account/org | e.g. `https://{owner}@github.com/{owner}/{repo}.git` |
| `{author}` | repo-local identity | `user.name` + `user.email` set per repo |

## Core principles

1. **Never commit directly to `{default}`.** Work happens on topic branches.
2. **Merge commits are normal.** The "merge bubbles" in the graph are exactly what makes git
   fast and conflict resolution cheap. This is industry standard.
3. **Rebase is the slow path, not merge.** Rebase replays every commit; merge resolves
   conflicts once. For bulk content batches, merge is dramatically faster.
4. **Push frequently.** Small drift = trivial conflicts; big drift = painful ones. Frequency
   is the cheapest conflict insurance.
5. **Pull at session start, push at session end.** Non-negotiable with multiple PCs.

## Branch naming

`<type>/<kebab-slug>` where `<type>` is a Conventional-Commits type:
`feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `perf`, `ci`, `style`, `build`.
Keep slugs short and descriptive (e.g. `feat/add-data-pack-skill`, `docs/sync-readme`).

For multi-PC content batches, prefix with PC identifier:
`content/florida-batch`, `content/texas-batch` — never share a branch name across PCs.

## Commit messages

Short imperative subject that **describes the actual change** (never a generic "update").
One line per logical change; body optional for multi-part commits.

**Example:**
```
feat(content): add water damage money pages for Florida

- 47 location pages with unique content
- Schema markup for all service pages
- Internal links verified
```

---

## Procedure — COMMIT (on a topic branch)

1. If on `{default}`, create the branch first:
   `git checkout -b <type>/<slug>` named for the actual change.
2. Inspect: `git status` and `git diff` — know exactly what is landing.
3. Stage and commit: `git add -A` then a descriptive imperative message.
4. If the commit fails (hooks reject), fix the cause and make a **new** commit — do not amend
   or bypass hooks.

---

## Procedure — PUBLISH (commit → merge → push)

### Single-branch (GitHub Flow — fastest for most teams)

```
main ──●────●────●────●────●──
        \  / \  /  \  /
    [PC1]● [PC2]● [PC3]●   ← merge --no-ff, delete branch after
```

1. **Commit pending work** on its topic branch.
2. **Switch to `{default}` and pull latest:**
   `git checkout {default}` → `git pull {remote} {default}`
3. **Merge the topic branch** (creates a merge commit):
   `git merge --no-ff <topic-branch> -m "merge: <descriptive message>"`
4. **Push `{default}`:**
   `git push {remote} {default}`
5. **Clean up:** delete the merged topic branch:
   `git branch -d <topic-branch>`
6. **Repeat** for any other topic branches (merge each into `{default}`).

**Why `--no-ff`?** The merge commit documents which branch was integrated and when.
It's the industry standard and makes the history readable without slowing anything down.

### Two-branch (with `{dev}` integration layer)

```
main ──●────●────●────●────●──
        \        \        /
    [topic]●  [topic]●  [topic]●  ← merged into dev first
         \        \        /
dev ──●───●────●───●────●───●──
```

1. **Commit** on topic branches cut from `{dev}`.
2. **Merge topic → `{dev}`:**
   `git checkout {dev}` → `git pull {remote} {dev}` → `git merge --no-ff <topic>`
3. **Push `{dev}`:** `git push {remote} {dev}`
4. **When ready to release:** merge `{dev}` → `{default}`:
   `git checkout {default}` → `git pull {remote} {default}` → `git merge --no-ff {dev}` → `git push {remote} {default}`
5. **Clean up** merged topic branches.

---

## Procedure — SYNC (multi-PC / shared remote)

GitHub is the single source of truth when several machines work the same repo:

### Session start
```bash
git pull {remote} {default}          # always start from latest
git checkout {default}               # ensure on default branch
git fetch {remote}                   # get all remote branches
```

### During work
- **One topic branch per PC per batch** — never share branch names across PCs.
- **Push immediately after merge** — keeps drift small.
- **Conflicts: report, never auto-resolve.** List the conflicted files, ask how to resolve,
  and let the user decide.

### Session end
```bash
# Merge any remaining topic branches (same as PUBLISH procedure)
git checkout {default}
git merge --no-ff <topic-branch>
git push {remote} {default}
git branch -d <topic-branch>        # only if merged successfully
```

---

## Windows performance optimizations

For repos with 10K+ files on Windows, these make a massive difference:

```bash
# Exclude repo from Windows Defender (biggest single speedup)
# Run PowerShell as Admin:
Add-MpPreference -ExclusionPath "D:\path\to\repo"

# Enable filesystem monitor (near-instant git status)
git config core.fsmonitor true

# Enable untracked file cache
git config core.untrackedcache true

# Schedule background maintenance (prefetch, gc, commit-graph)
git maintenance start
```

### Commit habits for speed
- **Fewer, larger commits** — 5 commits of 40 files beats 40 commits of 5 files for
  every operation that walks history.
- **Keep `package-lock.json` ignored** — lockfile churn on Windows is slow.
- **Bulk content batches** — merge is dramatically faster than rebase for 20+ commits
  of `.md` files.

---

## Commands

```bash
# Start work
git checkout -b <type>/<slug>

# Inspect
git status; git diff

# Commit
git add -A && git commit -m "<imperative subject>"

# Sync before merge
git checkout {default} && git pull {remote} {default}

# Merge (the fast path)
git merge --no-ff <topic-branch> -m "merge: <message>"

# Publish
git push {remote} {default}

# Clean up
git branch -d <topic-branch>

# Verify graph
git log --oneline --graph --all -20
```

---

## Checklist (done when)

- [ ] No commit was ever made directly on `{default}`.
- [ ] Every branch follows `<type>/<kebab-slug>`.
- [ ] Each commit subject is imperative and describes the actual change.
- [ ] Topic branches merged with `--no-ff` (merge commits present).
- [ ] Only `{default}` (and optionally `{dev}`) pushed to `{remote}`.
- [ ] Merged topic branches deleted after publishing.
- [ ] `git pull` at session start, `git push` at session end.
- [ ] Multiple PCs: no shared branch names across machines.

---

## Notes & gotchas

- **Merge commits are not "bad."** They're the industry standard. The "merge bubbles"
  you see in `git log --graph` are exactly what makes git fast and conflict resolution cheap.
- **Rebase is the slow path.** It replays every commit, which is painful for:
  - Large diffs (20+ commits replayed one by one)
  - Windows filesystem (every file touched = Windows Defender scan)
  - Multi-PC workflows (rebase breaks anyone else who has the branch)
- **Never rebase pushed branches** — with multiple PCs, rebasing anything on origin
  breaks everyone else.
- **Never `git push --force` `{default}`.**
- **Do not amend or drop commits** that are already pushed and shared.
- **Keep repo docs (README/CHANGELOG) in sync** in the same branch.
- **Windows note:** `LF will be replaced by CRLF` warnings are informational (autocrlf);
  they do not block the commit.
- **`--ff-only` is optional.** If you prefer linear history for specific cases, you can
  use `git merge --ff-only` when the branch is a strict descendant. But `--no-ff` is
  the default and faster for most workflows.
