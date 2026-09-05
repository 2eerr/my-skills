# My Skills

A personal **skills hub** — reusable [OpenCode](https://opencode.ai) skills that install into
any project (or globally) with the official [`skills`](https://github.com/vercel-labs/skills)
CLI. This repo is the single source of truth; it's pure Markdown (no build, no dependencies).

The skills encode a repeatable system for building **programmatic local-SEO / rank-and-rent
lead-gen sites** (any service niche). They were distilled from the reference specs in [`docs/`](docs).

## Skills

| Skill | What it does |
|---|---|
| [`programmatic-seo-autopilot`](skills/programmatic-seo-autopilot/SKILL.md) | Orchestrator: write every missing page by hand, one at a time, honoring the tracker (claim a region → locations first → region page last). |
| [`programmatic-seo-content-writing`](skills/programmatic-seo-content-writing/SKILL.md) | Content standard: ≥90% uniqueness, entity floors, expert voice, markdown/UI formatting, quality gates. |
| [`programmatic-seo-technical-seo`](skills/programmatic-seo-technical-seo/SKILL.md) | Technical & on-page SEO: URLs, titles/meta, JSON-LD matrix, sitemaps, Core Web Vitals, local SEO. |
| [`programmatic-seo-keyword-architecture`](skills/programmatic-seo-keyword-architecture/SKILL.md) | Keyword patterns per page type, tiers, and the related-services internal-link map. |
| [`programmatic-seo-service-catalog`](skills/programmatic-seo-service-catalog/SKILL.md) | Model the service/subservice list, categories, routing, hub/money specs, batch order. |
| [`programmatic-seo-location-model`](skills/programmatic-seo-location-model/SKILL.md) | Geography hierarchy, demand filter, and content-gated publishing rules. |
| [`programmatic-seo-data-packs`](skills/programmatic-seo-data-packs/SKILL.md) | Gather verified per-location local facts before writing. |
| [`programmatic-seo-design-system`](skills/programmatic-seo-design-system/SKILL.md) | Theme-able premium UI system: tokens, typography, components, accessibility. |
| [`programmatic-seo-deployment`](skills/programmatic-seo-deployment/SKILL.md) | Static build = publish (content gate), delivery checks, upload, cache headers. |

## Install

Requires [Node.js](https://nodejs.org) (for `npx`). No `npm install` needed — the CLI runs on demand.

```bash
# All skills, globally (available in every project)
npx skills add 2eerr/my-skills -g -a opencode

# Specific skills only
npx skills add 2eerr/my-skills --skill programmatic-seo-autopilot --skill programmatic-seo-content-writing -g -a opencode

# Into the current project only (writes to .agents/skills/, commit it with that project)
npx skills add 2eerr/my-skills -a opencode

# Preview what's in the repo without installing
npx skills add 2eerr/my-skills --list
```

> **Private repo:** the CLI reuses your existing git auth (Windows Credential Manager / `gh` /
> SSH), so no extra login is required.

## Update

After the maintainer edits and pushes new versions:

```bash
npx skills update -g -y     # refresh all global skills
npx skills update -p -y     # refresh project-scoped skills
npx skills list -g          # see what's installed
```

Updates take effect in **new** OpenCode sessions (no hot-reload).

## skills CLI reference

The [`skills`](https://github.com/vercel-labs/skills) CLI (the tool behind [skills.sh](https://skills.sh),
current v1.5.23) runs on demand with `npx skills …` — no install. Examples use this repo
(`2eerr/my-skills`); substitute any `owner/repo`.

### `skills add <source>` — install skills

**Source formats**
```bash
npx skills add 2eerr/my-skills                                   # GitHub shorthand (owner/repo)
npx skills add https://github.com/2eerr/my-skills                # full GitHub URL
npx skills add https://github.com/2eerr/my-skills/tree/main/skills/programmatic-seo-autopilot  # direct path to one skill
npx skills add https://gitlab.com/org/repo                       # GitLab URL
npx skills add git@github.com:2eerr/my-skills.git                # any git URL (SSH)
npx skills add ./my-local-skills                                 # local path
npx skills add https://example.com/download/my-skill             # direct SKILL.md / .zip / .tar(.gz) download
```

**Options**

| Option | Description |
|---|---|
| `-g, --global` | Install to the user directory instead of the project |
| `-a, --agent <agents...>` | Target specific agents (e.g. `opencode`, `claude-code`, `cursor`) |
| `-s, --skill <skills...>` | Install specific skills by name (`'*'` = all) |
| `-l, --list` | List available skills without installing |
| `--copy` | Copy files instead of symlinking into agent dirs |
| `-y, --yes` | Skip all confirmation prompts |
| `--all` | Install all skills to all agents, no prompts |
| `--full-depth` | Also discover `SKILL.md` outside the standard container dirs |

**Examples**
```bash
npx skills add 2eerr/my-skills --list                            # preview, no install
npx skills add 2eerr/my-skills -g -a opencode                    # all, global, OpenCode
npx skills add 2eerr/my-skills -a opencode                       # all, into current project
npx skills add 2eerr/my-skills --skill programmatic-seo-autopilot --skill programmatic-seo-content-writing -g -a opencode
npx skills add 2eerr/my-skills --skill '*' -a claude-code        # all skills to one agent
npx skills add 2eerr/my-skills --agent '*' --skill programmatic-seo-deployment   # one skill to all agents
npx skills add 2eerr/my-skills --all -y                          # everything to every agent (CI-friendly)
npx skills add owner/repo --skill "Convex Best Practices"        # names with spaces must be quoted
```

**Private repos** — same command; the CLI reuses the auth already configured for that URL
(Git credential helper → `gh` → SSH). You can also pass an explicit token via `GITHUB_TOKEN` / `GH_TOKEN`.

### `skills use <source>` — run one skill without installing
```bash
npx skills use 2eerr/my-skills@programmatic-seo-autopilot | claude
npx skills use 2eerr/my-skills --skill programmatic-seo-autopilot --agent opencode
```
Resolves the source like `add`, writes the skill to a temp dir, and prints the generated prompt
to stdout (or starts the agent interactively with `--agent`).

### `skills list` (`ls`) — show installed skills
```bash
npx skills list                       # project + global
npx skills ls -g                      # global only
npx skills ls -a opencode -a cursor   # filter by agent
```

### `skills find [query]` — search the ecosystem
```bash
npx skills find                       # interactive (fzf-style)
npx skills find typescript            # by keyword
npx skills find react --owner vercel  # within an owner/org
```

### `skills update [skills...]` — pull latest versions
```bash
npx skills update                     # all (interactive scope prompt)
npx skills update my-skill            # one (or several by name)
npx skills update -g                  # global only
npx skills update -p                  # project only
npx skills update -y                  # non-interactive (auto-detect scope)
```

| Option | Description |
|---|---|
| `-g, --global` | Only global skills |
| `-p, --project` | Only project skills |
| `-y, --yes` | Skip scope prompt (auto-detect: project if in one, else global) |
| `[skills...]` | Specific skills by name instead of all |

### `skills remove [skills]` (`rm`) — uninstall
```bash
npx skills remove                              # interactive picker
npx skills remove programmatic-seo-deployment  # by name
npx skills remove --global my-skill            # from global scope
npx skills remove --agent cursor my-skill      # from specific agents
npx skills remove --all                        # everything, no confirm
npx skills rm my-skill                         # 'rm' alias
```

| Option | Description |
|---|---|
| `-g, --global` | Remove from global (`~/`) instead of project |
| `-a, --agent` | From specific agents (`'*'` = all) |
| `-s, --skill` | Specify skills (`'*'` = all) |
| `-y, --yes` | Skip prompts |
| `--all` | Shorthand for `--skill '*' --agent '*' -y` |

### `skills init [name]` — scaffold a new SKILL.md
```bash
npx skills init            # ./SKILL.md in the current dir
npx skills init my-skill   # my-skill/SKILL.md
```

### Installation scope & method

| Scope | Flag | Location (OpenCode) | Use |
|---|---|---|---|
| Project | (default) | `./<agent>/skills/` → `.agents/skills/` | committed with the project, shared with team |
| Global | `-g` | `~/<agent>/skills/` → `~/.config/opencode/skills/` | available in all projects |

**Method:** *Symlink* (default/recommended — one canonical copy, instant updates) or *Copy*
(`--copy`, independent copies; use when symlinks are blocked, e.g. Windows without Developer
Mode — then re-run `npx skills update` after each push).

## Contribute / edit

```bash
git clone https://2eerr@github.com/2eerr/my-skills.git
cd my-skills
# edit or add skills under skills/<name>/SKILL.md
git add -A && git commit -m "..." && git push
```

### Skill file rules
- One folder per skill: `skills/<name>/SKILL.md` (folder name **must** equal the frontmatter `name`).
- `name`: 1–64 chars, lowercase alphanumeric + single hyphens (`^[a-z0-9]+(-[a-z0-9]+)*$`).
- `description`: 1–1024 chars, specific — the agent uses it to decide when to load the skill.
- Recognized frontmatter only: `name`, `description`, `license`, `compatibility`, `metadata`.
- File must be exactly `SKILL.md` (all caps).
- Validate locally: `npx skills add . --list`.

## Repo layout

```
my-skills/
├── README.md
├── plan.md            # the hosting/CLI decision record
├── docs/              # reference specs the skills were distilled from (a sample niche)
└── skills/            # the installable skills (auto-discovered by the CLI)
    └── programmatic-seo-*/SKILL.md
```

## Environment variables & notes

| Variable | Description |
|---|---|
| `DISABLE_TELEMETRY` / `DO_NOT_TRACK` | Disable the CLI's anonymous usage telemetry |
| `GITHUB_TOKEN` / `GH_TOKEN` | Explicit token for private GitHub downloads / update checks |
| `INSTALL_INTERNAL_SKILLS=1` | Show/install skills marked `metadata.internal: true` |
| `SKILLS_DOWNLOAD_MAX_BYTES` / `SKILLS_EXTRACT_MAX_BYTES` / `SKILLS_EXTRACT_MAX_FILES` | Size/count limits for direct-download & archive sources |

- **Other agents:** swap `-a opencode` for `claude-code`, `cursor`, `codex`, etc., or `-a '*'`
  for all — the CLI supports 70+ agents.
- **Updates take effect in new sessions** (no hot-reload).
- **Naming collisions:** skill names must be unique across global + project locations.
- Full upstream reference: [npm `skills`](https://www.npmjs.com/package/skills) ·
  [GitHub](https://github.com/vercel-labs/skills) · [skills.sh](https://skills.sh).
