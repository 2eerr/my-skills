# AGENTS.md — AI working instructions for this repo

This file is auto-loaded by OpenCode (and other coding agents) as project instructions.
Follow it whenever working in this repository.

## What this repo is

A **skills hub**: reusable **agent skills** (standard `SKILL.md` — OpenCode, Freebuff, Claude
Code, Codex, Cursor, and 70+ other AI IDEs/agents), installed with the official `skills` CLI
(`npx skills`). It is **pure Markdown** — no build step, no dependencies, **no `package.json`**.
Skills must stay **agent-agnostic**: never bind a skill's content to one specific IDE.
GitHub (`2eerr/my-skills`) is the single source of truth.

## Reading protocol — what to read before you act

**Start here, then follow the routing table.** Every task maps to a doc (or set of docs)
that defines "done" for that task.

| If you are going to… | Read these first | Then act on |
|---|---|---|
| **Add/update/remove a skill** | This file (AGENTS.md) → `README.md` → `CHANGELOG.md` | `skills/<name>/SKILL.md` |
| **Edit this file (AGENTS.md)** | This file → `skills/agents-md/SKILL.md` | `AGENTS.md` |
| **Sync docs with code** | `README.md` + `CHANGELOG.md` + `skills/` directory | Update stale sections |
| **Validate skill discovery** | This file → Commands section | `INSTALL_INTERNAL_SKILLS=1 npx skills add . --list` |
| **Install skills globally** | `README.md` → Install section | `npx skills add 2eerr/my-skills -g -a '*'` |

**Action rule:** read the relevant doc, then make the change, then update the matching doc
if your change affects documented behavior. Never leave a task "done" with its doc stale.

## Documentation map

| Doc | What it covers | When to read it |
|---|---|---|
| `AGENTS.md` (this file) | AI working instructions, golden rules, skill spec, commands, conventions, git rules | First read — the entry point to the whole repo |
| `README.md` | Human-facing overview, skills table, install/update commands, CLI reference | Before installing skills or sharing the repo |
| `CHANGELOG.md` | Chronological record of skill/doc changes | Before committing changes to see what's documented |
| `skills/*/SKILL.md` | Individual skill instructions (19 skills) | When working on a specific skill |
| `templates/SKILL.template.md` | Scaffold for new skills | When creating a new skill |
| `source/docs/` | Niche-specific reference files (water damage site) | When creating/updating skills from real implementations |

## Decisions (locked)

- **CLI:** use ONLY the official `skills` CLI (vercel-labs/skills, the tool behind skills.sh) for
  every skill action — no custom install scripts, no junctions, no `install.ps1`, no
  `package.json` tooling.
- **Hosting:** GitHub repo = single source of truth, versioned with git; installs pull from it
  (`npx skills add 2eerr/my-skills …`), updates via `npx skills update`.
- **Visibility:** repo stays public, but every skill is `internal: true` so nothing registers on
  skills.sh via install telemetry.

## Layout

```
my-skills/
├── AGENTS.md            # this file — AI instructions
├── README.md            # human-facing overview + install/update commands
├── CHANGELOG.md         # chronological record of skill/doc changes (keep in sync)
├── templates/           # scaffolds (NOT discovered by the CLI)
│   └── SKILL.template.md
├── skills/              # the installable skills (auto-discovered by the CLI)
│   └── <skill-name>/SKILL.md
└── source/              # niche-specific reference files from a real implementation (gitignored)
    ├── docs/            # domain manuals + content pipeline docs (water damage site)
    ├── scripts/         # build, validation, and generation scripts (water damage site)
    └── temp/            # ad-hoc throwaway helpers (one-off audits, fixers)
```

## Golden rules

1. **Skills must stay niche-agnostic.** Do NOT hardcode a brand, domain, product, fixed
   service list, analytics ID, or niche-specific examples as *bindings*. Use `{placeholders}`
   (`{Brand}`, `{domain}`, `{region}`, `{location}`, `{service}`) and a "read the project
   config" step. Concrete niche examples are allowed only as clearly-labeled *illustrations*.
2. **One folder per skill.** `skills/<name>/SKILL.md`; the folder name **must equal** the
   frontmatter `name`.
3. **Filename is exactly `SKILL.md`** (all caps).
4. **No scripts that write content** into skills; scripts may only scaffold/validate.
5. **Skills must stay agent-agnostic.** They are used across many AI IDEs (mostly OpenCode and
   Freebuff, but also Claude Code, Codex, Cursor, …). Write rules in generic terms — never bind
   a skill's content to one specific agent/IDE.

### source/ → skill relationship

The `source/` directory holds **niche-specific reference files from a real implementation**
(water damage restoration site). These are the *source material* the skills were distilled from,
but they are **not** the skills themselves. When creating or updating a skill from `source/`
files: extract the **general pattern, rules, and workflow** — then generalize it with
`{placeholders}` so it applies to any niche. Never carry brand names, domain-specific service
lists, hardcoded analytics IDs, or location-specific data into a skill. The `source/` files are
reference; the skills are the reusable abstraction.

## Skill file spec (frontmatter)

- `name`: 1–64 chars, lowercase alphanumeric + single hyphens, regex `^[a-z0-9]+(-[a-z0-9]+)*$`.
- `description`: 1–1024 chars, **specific** — the agent uses it to decide when to load the
  skill. State what it does AND when to use it (triggers).
- Recognized keys only: `name`, `description`, `license`, `compatibility`, `metadata`.
- **CLI discovery:** skills are found under `skills/` (flat or up to 2 category levels deep), the
  repo root, or common agent dirs — the `skills/<name>/SKILL.md` layout is the standard one.
  `--full-depth` additionally scans outside those container dirs.
- **Every skill MUST carry `metadata: internal: true`** in its frontmatter — it hides the skill
  from `skills` CLI discovery and normal installs, so nothing registers on skills.sh via install
  telemetry (GitHub stays public). Validation and installs therefore require
  `INSTALL_INTERNAL_SKILLS=1`.
- **YAML gotcha:** if `description` contains a colon-space (`: `), **wrap the whole value in
  double quotes** — otherwise the CLI throws "Nested mappings are not allowed in compact
  mappings". Example: `description: "… the gate (unique: true) that …"`.

## Commands

```bash
INSTALL_INTERNAL_SKILLS=1 npx skills add . --list                    # validate discovery (run after ANY skill change)
INSTALL_INTERNAL_SKILLS=1 npx skills add 2eerr/my-skills -g -a '*'           # install all, globally, into every detected agent
INSTALL_INTERNAL_SKILLS=1 npx skills add 2eerr/my-skills -g -a opencode -a claude-code   # pick agents
INSTALL_INTERNAL_SKILLS=1 npx skills add 2eerr/my-skills -a '*'              # into current project (.agents/skills/)
INSTALL_INTERNAL_SKILLS=1 npx skills add 2eerr/my-skills --skill <name> -g -a '*'        # one skill
npx skills update -g -y                                   # refresh global skills after a push
npx skills list -g                                        # what's installed
npx skills remove <name>                                  # remove (project scope) / add -g for global
```

- **Skills are `internal: true`** — plain `npx skills add . --list` finds none by default; that is
  the expected, delisted state. Prefix `INSTALL_INTERNAL_SKILLS=1` for anything that must see or
  install them. **PowerShell/cmd (Windows):** the `VAR=1 command` prefix is bash-only — set the
  variable first (`$env:INSTALL_INTERNAL_SKILLS = '1'` in PowerShell, `set INSTALL_INTERNAL_SKILLS=1&& …`
  in cmd), then run the plain command. Git Bash uses the prefixed form as documented.
- **Agents:** prefer `-a '*'` (all detected) or an explicit agent list — the skills are not
  OpenCode-only. **Freebuff** isn't in the CLI's agent list yet: install with `-a universal`
  (project → `.agents/skills/`) and point Freebuff at the skills from the project's `AGENTS.md`.
- **Telemetry:** keep `DISABLE_TELEMETRY=1` set on maintainer machines (persistent user env var,
  e.g. PowerShell: `[Environment]::SetEnvironmentVariable('DISABLE_TELEMETRY','1','User')`) so
  own installs are never reported to skills.sh.

## Shorthand commands (trigger phrases)

When the user types one of these on its own, perform the action immediately — don't ask for
confirmation. These are repo-workflow shortcuts, not `skills` CLI commands.

| Trigger | Do this |
|---|---|
| `git!` | **Descriptive commit (on a branch).** If on `main`, first create a feature branch named for the change (see Conventions → Branching) — **never commit directly to `main`.** Inspect `git status` + `git diff`, stage all changes (`git add -A`), and commit with a **specific imperative message that describes the actual change** (never a generic "update"). |
| `push!` | **Commit, integrate, publish, then clean up `main` — in this order, keeping history linear:** (1) run `git!` (commit pending work on its feature branch); (2) for **every other local branch except `main`**, rebase it onto `main` then fast-forward `main` onto it (`git rebase main <branch>` → `git checkout main && git merge --ff-only <branch>`) — **no merge commits**; (3) push **`main`** to `origin` (`git push origin main`); (4) **delete only the local branches that merged successfully into `main`** (`git branch --merged main`, excluding `main` itself) — never delete unmerged branches. |
| `docs!` | **Sync the docs with the real content.** Update every doc so it matches the current repo — the README skills table, the "Current skills" list here, `CHANGELOG.md`, and the local-only `source/docs/` — so all fields, lists, commands, and counts reflect the actual files. Re-validate with `INSTALL_INTERNAL_SKILLS=1 npx skills add . --list`. |
| `i!` | **Instruction only — do NOT act.** Don't perform the described action. Instead, add it as a rule/instruction to the relevant `.md` file (usually `AGENTS.md`, or the matching skill/doc), then stop. |
| `ii!` | **Implement the requested things AND record them in the docs.** When the user types `ii!` (typically right after a feature/fix request), implement in code whatever was asked **and** add/update the relevant `.md` files (`AGENTS.md` + `README.md` + `source/docs/*.md` where applicable) so the new behavior is documented as instructions. Place the instruction in the most relevant file — `AGENTS.md` for workflow/commands, `README.md` for project-level, `source/docs/*.md` for domain-specific. Code and docs must stay in sync — never leave a task "done" with its doc stale. |
| `clean!` | **Merge & prune branches (no push).** (1) run `git!` (commit pending work on its feature branch); (2) integrate **every other local branch** into `main` — rebase onto `main` then fast-forward (`git rebase main <branch>` → `git checkout main && git merge --ff-only <branch>`), no merge commits; (3) **delete only the local branches that merged successfully** into `main` (`git branch --merged main`, excluding `main`) — branch names only, never their code (use `git branch -d`, never `-D`, so unmerged work can't be lost). |

Notes:
- `i!` takes precedence: if a message starts with `i!`, only record the instruction in the right
  `.md` file — never execute it.
- `ii!` is the opposite: implement **and** record. If a message starts with `ii!`, make the
  code/content changes **and** update the relevant docs so the new behavior is documented.
- `git!`/`push!` follow the Conventions below (commit to a **feature branch, never `main`**;
  pinned `2eerr` remote; author `hammad <hello@hammad.com>`).
- `push!` is the sanctioned way to land work on `main`: it rebases feature branches onto `main`
  and fast-forwards (a linear merge, not a merge commit), then pushes `main`. Direct commits on
  `main` stay forbidden.
- `clean!` is `push!` without the push: it commits, integrates every branch into `main`, then
  prunes only the merged branch names — the code always stays in `main`.
- `docs!` is the mechanism that keeps this repo's documentation from drifting — run it after any
  add/update/remove of a skill, and it should leave nothing out of sync.

## Workflow — ADD a skill

1. Copy `templates/SKILL.template.md` → `skills/<new-name>/SKILL.md`.
2. Set `name` (== folder) and a specific `description` (quote it if it has `: `).
3. Write the body: purpose, when-to-use, the actionable rules/steps, commands, checklist.
4. Keep it niche-agnostic (Golden rule 1).
5. **Validate:** `INSTALL_INTERNAL_SKILLS=1 npx skills add . --list` — must appear with no parse
   error (and be absent without the flag — that proves it stays off skills.sh).
6. Add a row to the **README** skills table.
7. Add a **CHANGELOG** entry.
8. Commit + push (see Conventions).

## Workflow — UPDATE a skill

1. Edit `skills/<name>/SKILL.md`.
2. Re-validate with `INSTALL_INTERNAL_SKILLS=1 npx skills add . --list`.
3. Bump the CHANGELOG entry.
4. Commit + push. Tell users to run `npx skills update -g -y` (takes effect in new sessions).

## Workflow — REMOVE a skill

1. Delete the `skills/<name>/` folder.
2. Remove its README table row + note the removal in CHANGELOG.
3. Validate, commit, push.

## Conventions

- **Git:** **never commit directly to `main`.** For any change, create a feature branch named for
  that change (see **Branching**), commit there, and integrate via a **squash- or rebase-merge**
  (never a merge commit). **Only `main` is ever pushed to `origin`** — feature branches stay
  local until they are fast-forwarded into `main`. The remote is pinned to the
  `2eerr` account (`https://2eerr@github.com/2eerr/my-skills.git`) so pushes never prompt for an account.
- **Branching (industry standard):** `<type>/<kebab-slug>`, where `<type>` is a Conventional-Commits
  type — `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `perf`, `ci`, `style`, `build`. Keep the
  slug short and descriptive, e.g. `feat/add-data-pack-skill`, `docs/sync-readme-cli`,
  `fix/skill-frontmatter-colon`. Optionally prefix an issue number (`feat/123-…`). `main` stays
  protected; work lands from feature branches via PRs that are squash-/rebase-merged.
- **Linear history (always):** the git graph must stay a single straight line — **no merge
  commits.** Rebase feature branches onto `main` and integrate with `git merge --ff-only`; use
  `git pull --rebase` (never a merging pull). Avoid `--no-ff`.
- **Author:** local `user.name=hammad`, `user.email=hello@hammad.com` (already set for this repo).
- **Commit messages:** short imperative subject; one line per logical change.
- **Never** add a `package.json`, lockfile, or build tooling — this repo has no code.
- **Never** hand-edit generated files; keep README + CHANGELOG in sync with `skills/`.
- After any change under `skills/`, always run `INSTALL_INTERNAL_SKILLS=1 npx skills add . --list`
  before committing.

## GitHub sync (multi-PC workflow)

**GitHub is the single source of truth.** Multiple PCs work from the same GitHub repo.

### Pull at session start

Before doing any work, pull the latest changes:

```bash
git pull origin main
```

Always start new branches from the latest `main`. Pulling is safe and expected — never skip it.

### Push at session end

After committing on a feature branch, push when ready:

```bash
git push origin main
```

Only `main` is ever pushed to `origin`. Feature branches stay local until merged.

### Conflict resolution

If `git pull` results in a merge conflict:
1. Report the conflicted files to the user
2. Ask how to resolve (accept theirs, accept ours, or manual merge)
3. Never auto-resolve conflicts — the user decides

### Pinned remote URL

The remote URL embeds the GitHub username so Credential Manager picks the right account:

```bash
git remote set-url origin https://2eerr@github.com/2eerr/my-skills.git
```

This is a per-clone, local-only setting — other PC clones keep their own origin URL.

## Docs-sync rule

**Code and docs must not drift.** When you change skills, update the matching docs in the same
change:

- **Add/update/remove a skill** → update README skills table + CHANGELOG + "Current skills" list
- **Edit AGENTS.md** → verify all sections match current repo state
- **Edit README.md** → verify install commands, skills table, and layout match current files

**Never leave a task "done" with its doc stale.** The docs are the knowledge base — if they
describe something that no longer exists, the next agent will follow outdated instructions.

## Quality gates (delivery gate)

Before any change is "done":

1. **Validate:** `INSTALL_INTERNAL_SKILLS=1 npx skills add . --list` — must discover all skills
   with no parse errors.
2. **README sync:** skills table matches `skills/` directory, install commands work.
3. **CHANGELOG sync:** entry documents the change.
4. **AGENTS.md sync:** "Current skills" list matches actual skills.

## Context-gathering rules

When the user asks to "read" or "gather context" from the repo:

1. Read `AGENTS.md` (this file) first.
2. Read `README.md` for project overview.
3. Read any skill files referenced in the task.
4. **Gather context only** — do not take action unless explicitly asked.

The docs are a **knowledge base, not an automation script**. If a doc says something
that contradicts reality or the user's request, flag the discrepancy and ask — never
mechanically execute an outdated instruction.

## Post-task follow-up

After finishing any task, propose **5 contextually relevant next steps** drawn from the
project's current state. Suggestions only — never perform them unprompted. Each should be
a concrete, likely-next action.

## Quality bar for a good skill

- Description is specific enough that the agent loads it at the right moment (and only then).
- Body is actionable and self-contained — it works without opening the local-only `source/docs/`.
- Uses imperative, concrete language; no filler.
- Stays general (placeholders + config step), so it applies across many projects.
- Reasonable length; split into multiple skills when topics diverge.

## Current skills

`seo-autopilot`, `seo-content-writing`,
`seo-onpage-seo`, `seo-technical-seo`, `seo-schema`,
`seo-local-seo`, `seo-image-seo`,
`seo-keyword-architecture`, `seo-service-catalog`,
`seo-location-model`, `seo-data-packs`,
`seo-design-system`, `seo-deployment`, `seo-htaccess`, `astro-ssg`, `seo-site-blueprint`,
`git-workflow`, `tracker`, `agents-md`.
