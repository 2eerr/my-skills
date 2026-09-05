# AGENTS.md — AI working instructions for this repo

This file is auto-loaded by OpenCode (and other coding agents) as project instructions.
Follow it whenever working in this repository.

## What this repo is

A **skills hub**: reusable OpenCode skills, installed with the official `skills` CLI
(`npx skills`). It is **pure Markdown** — no build step, no dependencies, **no `package.json`**.
GitHub (`2eerr/my-skills`) is the single source of truth.

## Layout

```
my-skills/
├── AGENTS.md            # this file — AI instructions
├── README.md            # human-facing overview + install/update commands
├── CHANGELOG.md         # chronological record of skill/doc changes (keep in sync)
├── plan.md              # decision record (hosting + CLI choice)
├── templates/           # scaffolds (NOT discovered by the CLI)
│   └── SKILL.template.md
├── docs/                # reference specs the skills were distilled from (a sample niche)
└── skills/              # the installable skills (auto-discovered by the CLI)
    └── <skill-name>/SKILL.md
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

## Skill file spec (frontmatter)

- `name`: 1–64 chars, lowercase alphanumeric + single hyphens, regex `^[a-z0-9]+(-[a-z0-9]+)*$`.
- `description`: 1–1024 chars, **specific** — the agent uses it to decide when to load the
  skill. State what it does AND when to use it (triggers).
- Recognized keys only: `name`, `description`, `license`, `compatibility`, `metadata`.
- **YAML gotcha:** if `description` contains a colon-space (`: `), **wrap the whole value in
  double quotes** — otherwise the CLI throws "Nested mappings are not allowed in compact
  mappings". Example: `description: "… the gate (unique: true) that …"`.

## Commands

```bash
npx skills add . --list                                   # validate discovery (run after ANY skill change)
npx skills add 2eerr/my-skills -g -a opencode             # install all, globally
npx skills add 2eerr/my-skills -a opencode                # install into current project (.agents/skills/)
npx skills add 2eerr/my-skills --skill <name> -g -a opencode   # one skill
npx skills update -g -y                                   # refresh global skills after a push
npx skills list -g                                        # what's installed
npx skills remove <name>                                  # remove (project scope) / add -g for global
```

## Shorthand commands (trigger phrases)

When the user types one of these on its own, perform the action immediately — don't ask for
confirmation. These are repo-workflow shortcuts, not `skills` CLI commands.

| Trigger | Do this |
|---|---|
| `git!` | **Descriptive commit (on a branch).** If on `main`, first create a feature branch named for the change (see Conventions → Branching) — **never commit directly to `main`.** Inspect `git status` + `git diff`, stage all changes (`git add -A`), and commit with a **specific imperative message that describes the actual change** (never a generic "update"). |
| `push!` | **Commit, integrate, then publish `main` — in this order, keeping history linear:** (1) run `git!` (commit pending work on its feature branch); (2) for **every other local branch except `main`**, rebase it onto `main` then fast-forward `main` onto it (`git rebase main <branch>` → `git checkout main && git merge --ff-only <branch>`) — **no merge commits**; (3) push **`main`** to `origin` (`git push origin main`). |
| `docs!` | **Sync the docs with the real content.** Update every doc so it matches the current repo — the README skills table, the "Current skills" list here, `CHANGELOG.md`, and anything under `docs/` — so all fields, lists, commands, and counts reflect the actual files. Re-validate with `npx skills add . --list`. |
| `i!` | **Instruction only — do NOT act.** Don't perform the described action. Instead, add it as a rule/instruction to the relevant `.md` file (usually `AGENTS.md`, or the matching skill/doc), then stop. |

Notes:
- `i!` takes precedence: if a message starts with `i!`, only record the instruction in the right
  `.md` file — never execute it.
- `git!`/`push!` follow the Conventions below (commit to a **feature branch, never `main`**;
  pinned `2eerr` remote; author `hammad <hello@hammad.com>`).
- `push!` is the sanctioned way to land work on `main`: it rebases feature branches onto `main`
  and fast-forwards (a linear merge, not a merge commit), then pushes `main`. Direct commits on
  `main` stay forbidden.
- `docs!` is the mechanism that keeps this repo's documentation from drifting — run it after any
  add/update/remove of a skill, and it should leave nothing out of sync.

## Workflow — ADD a skill

1. Copy `templates/SKILL.template.md` → `skills/<new-name>/SKILL.md`.
2. Set `name` (== folder) and a specific `description` (quote it if it has `: `).
3. Write the body: purpose, when-to-use, the actionable rules/steps, commands, checklist.
4. Keep it niche-agnostic (Golden rule 1).
5. **Validate:** `npx skills add . --list` — must appear with no parse error.
6. Add a row to the **README** skills table.
7. Add a **CHANGELOG** entry.
8. Commit + push (see Conventions).

## Workflow — UPDATE a skill

1. Edit `skills/<name>/SKILL.md`.
2. Re-validate with `npx skills add . --list`.
3. Bump the CHANGELOG entry.
4. Commit + push. Tell users to run `npx skills update -g -y` (takes effect in new sessions).

## Workflow — REMOVE a skill

1. Delete the `skills/<name>/` folder.
2. Remove its README table row + note the removal in CHANGELOG.
3. Validate, commit, push.

## Conventions

- **Git:** **never commit directly to `main`.** For any change, create a feature branch named for
  that change (see **Branching**), commit there, and integrate via a **squash- or rebase-merge**
  (never a merge commit). The remote is pinned to the
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
- After any change under `skills/`, always run `npx skills add . --list` before committing.

## Quality bar for a good skill

- Description is specific enough that the agent loads it at the right moment (and only then).
- Body is actionable and self-contained — it works without opening `docs/`.
- Uses imperative, concrete language; no filler.
- Stays general (placeholders + config step), so it applies across many projects.
- Reasonable length; split into multiple skills when topics diverge.

## Current skills

`programmatic-seo-autopilot`, `programmatic-seo-content-writing`,
`programmatic-seo-technical-seo`, `programmatic-seo-keyword-architecture`,
`programmatic-seo-service-catalog`, `programmatic-seo-location-model`,
`programmatic-seo-data-packs`, `programmatic-seo-design-system`, `programmatic-seo-deployment`.
