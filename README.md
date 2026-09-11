# My Skills

A personal **skills hub** — reusable **agent skills** (standard `SKILL.md` format: works with
OpenCode, Freebuff, Claude Code, Codex, Cursor, Windsurf, Cline, and 70+ other AI IDEs/agents)
that install into any project (or globally) with the official
[`skills`](https://www.npmjs.com/package/skills) CLI — the npm package used for **every**
skill action here (install, update, list, find, remove, init).
This repo is the single source of truth; it's pure Markdown (no build, no dependencies).

The skills encode a repeatable system for building **local-SEO / rank-and-rent
lead-gen sites** (any service niche). They were distilled from the maintainer's reference specs
(kept local-only in `docs/`, not published with the repo).

## Skills

| Skill | What it does |
|---|---|
| [`seo-autopilot`](skills/seo-autopilot/SKILL.md) | Orchestrator: write every missing page by hand, one at a time, honoring the tracker (claim a region → locations first → region page last). |
| [`seo-site-blueprint`](skills/seo-site-blueprint/SKILL.md) | Phased build plan from zero: data → scaffold → pilot region → batched rollout → SEO layer → QA/launch → post-launch, with gates + scope discipline. |
| [`seo-content-writing`](skills/seo-content-writing/SKILL.md) | Content standard: ≥90% uniqueness, entity floors, expert voice, markdown/UI formatting, quality gates. |
| [`seo-onpage-seo`](skills/seo-onpage-seo/SKILL.md) | On-page: titles/meta, canonical, headings, internal links & link equity, OG/Twitter. |
| [`seo-technical-seo`](skills/seo-technical-seo/SKILL.md) | Technical: URL architecture, sitemaps, robots, Core Web Vitals, indexability, SEO check script. |
| [`seo-schema`](skills/seo-schema/SKILL.md) | JSON-LD structured data: base graph, per-page schema matrix, `@id` rules, FAQPage/review guards, LocalBusiness/NAP, validation. |
| [`seo-local-seo`](skills/seo-local-seo/SKILL.md) | Local: on-page local signals, GBP & citations, NAP consistency, E-E-A-T/off-page, local KPIs. |
| [`seo-image-seo`](skills/seo-image-seo/SKILL.md) | Images: self-hosting, SVG/WebP, width/height+alt+lazy, OG card, licensing & attribution. |
| [`seo-keyword-architecture`](skills/seo-keyword-architecture/SKILL.md) | Keyword patterns per page type, tiers, and the related-services internal-link map. |
| [`seo-service-catalog`](skills/seo-service-catalog/SKILL.md) | Model the service/subservice list, categories, routing, hub/money specs, batch order. |
| [`seo-location-model`](skills/seo-location-model/SKILL.md) | Geography hierarchy, demand filter, and content-gated publishing rules. |
| [`seo-data-packs`](skills/seo-data-packs/SKILL.md) | Gather verified per-location local facts before writing. |
| [`seo-design-system`](skills/seo-design-system/SKILL.md) | Theme-able premium UI system: tokens, typography, components, accessibility. |
| [`seo-deployment`](skills/seo-deployment/SKILL.md) | Static build = publish (content gate), delivery checks, upload, cache headers. |
| [`astro-ssg`](skills/astro-ssg/SKILL.md) | Astro mechanics: scaffold, getStaticPaths routes, Base layout, content rendering, components, build output + build-time performance at scale. |
| [`linear-git-workflow`](skills/linear-git-workflow/SKILL.md) | Commit/publish with linear history: feature branches `<type>/<slug>`, descriptive commits, rebase + `--ff-only`, push default branch. |
| [`tracker`](skills/tracker/SKILL.md) | TRACKER.md file format and generator-script contract: exact layout, section structure, status symbols, per-section line formats, and the input/output rules for producing the tracker from the content folder. |

## Install

Requires [Node.js](https://nodejs.org) (for `npx`). No `npm install` needed — the CLI runs on demand.

These skills are marked `internal: true` (kept off skills.sh), so every command below needs the
`INSTALL_INTERNAL_SKILLS=1` prefix. Target agents with `-a '*'` (every agent you have installed)
or list them explicitly (`-a opencode -a claude-code …`).

```bash
# All skills, globally, into every supported agent you have installed
INSTALL_INTERNAL_SKILLS=1 npx skills add 2eerr/my-skills -g -a '*'

# Only specific agents (e.g. OpenCode + Claude Code)
INSTALL_INTERNAL_SKILLS=1 npx skills add 2eerr/my-skills -g -a opencode -a claude-code

# Specific skills only
INSTALL_INTERNAL_SKILLS=1 npx skills add 2eerr/my-skills --skill seo-autopilot --skill seo-content-writing -g -a '*'

# Into the current project only (writes to .agents/skills/, commit it with that project)
INSTALL_INTERNAL_SKILLS=1 npx skills add 2eerr/my-skills -a '*'

# Preview what's in the repo without installing
INSTALL_INTERNAL_SKILLS=1 npx skills add 2eerr/my-skills --list
```

> **Freebuff (freebuff.com):** not in the CLI's supported-agent list yet. Install to the
> universal folder instead — `INSTALL_INTERNAL_SKILLS=1 npx skills add 2eerr/my-skills -a universal`
> (project → `.agents/skills/`, global → `~/.config/agents/skills/`) — and reference the skills
> from your project's `AGENTS.md` (e.g. "load the skills under `.agents/skills/` before SEO
> work"), since Freebuff reads `AGENTS.md`. The skills are plain Markdown, so they work in any
> agent that can read a `SKILL.md`.

> **Private repo:** the CLI reuses your existing git auth (Windows Credential Manager / `gh` /
> SSH), so no extra login is required.

## Update

After the maintainer edits and pushes new versions:

```bash
npx skills update -g -y     # refresh all global skills
npx skills update -p -y     # refresh project-scoped skills
npx skills list -g          # see what's installed
```

On a **new machine**: run the install once (any `npx skills add …` from above), then keep it
current with `npx skills update -g -y` after each push.

Updates take effect in **new** agent sessions (no hot-reload).

## skills CLI reference

All skill actions — **install, update, list, find, remove, and scaffolding** — are performed with
the [`skills`](https://www.npmjs.com/package/skills) npm package
([npm](https://www.npmjs.com/package/skills) · [GitHub](https://github.com/vercel-labs/skills) ·
the tool behind [skills.sh](https://skills.sh), current v1.5.23). It runs on demand via
`npx skills …` — no global install needed. Examples use this repo (`2eerr/my-skills`); substitute
any `owner/repo`. Because this repo's skills are `internal: true`, prefix the examples that
target it with `INSTALL_INTERNAL_SKILLS=1`.

### `skills add <source>` — install skills

**Source formats**
```bash
npx skills add 2eerr/my-skills                                   # GitHub shorthand (owner/repo)
npx skills add https://github.com/2eerr/my-skills                # full GitHub URL
npx skills add https://github.com/2eerr/my-skills/tree/main/skills/seo-autopilot  # direct path to one skill
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
npx skills add 2eerr/my-skills --skill seo-autopilot --skill seo-content-writing -g -a opencode
npx skills add 2eerr/my-skills --skill '*' -a claude-code        # all skills to one agent
npx skills add 2eerr/my-skills --agent '*' --skill seo-deployment   # one skill to all agents
npx skills add 2eerr/my-skills --all -y                          # everything to every agent (CI-friendly)
npx skills add owner/repo --skill "Convex Best Practices"        # names with spaces must be quoted
```

**Private repos** — same command; the CLI reuses the auth already configured for that URL
(Git credential helper → `gh` → SSH). You can also pass an explicit token via `GITHUB_TOKEN` / `GH_TOKEN`.

### `skills use <source>` — run one skill without installing
```bash
npx skills use 2eerr/my-skills@seo-autopilot | claude
npx skills use 2eerr/my-skills --skill seo-autopilot --agent opencode
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
npx skills remove seo-deployment  # by name
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

| Scope | Flag | Location (OpenCode / universal) | Use |
|---|---|---|---|
| Project | (default) | `.agents/skills/` | committed with the project, shared with team |
| Global | `-g` | `~/.config/opencode/skills/` / `~/.config/agents/skills/` | available in all projects |

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
- Every skill carries `metadata: internal: true` — hidden from skills.sh; installs need
  `INSTALL_INTERNAL_SKILLS=1`.
- File must be exactly `SKILL.md` (all caps).
- Validate locally: `INSTALL_INTERNAL_SKILLS=1 npx skills add . --list` (the skills are internal).

## Repo layout

```
my-skills/
├── README.md
├── AGENTS.md            # AI working instructions (rules, workflows, conventions)
├── CHANGELOG.md         # chronological record of skill/doc changes
├── templates/           # scaffolds (not discovered by the CLI)
├── skills/              # the installable skills (auto-discovered by the CLI)
│   ├── seo-*/SKILL.md
│   └── linear-git-workflow/SKILL.md
└── source/              # niche-specific reference files from a real implementation (gitignored)
    ├── docs/            # domain manuals + content pipeline docs (water damage site)
    ├── scripts/         # build, validation, and generation scripts (water damage site)
    └── temp/            # ad-hoc throwaway helpers (one-off audits, fixers)
```

## Environment variables & notes

| Variable | Description |
|---|---|
| `DISABLE_TELEMETRY` / `DO_NOT_TRACK` | Disable the CLI's anonymous usage telemetry |
| `GITHUB_TOKEN` / `GH_TOKEN` | Explicit token for private GitHub downloads / update checks |
| `INSTALL_INTERNAL_SKILLS=1` | Show/install skills marked `metadata.internal: true` |
| `SKILLS_DOWNLOAD_MAX_BYTES` / `SKILLS_EXTRACT_MAX_BYTES` / `SKILLS_EXTRACT_MAX_FILES` | Size/count limits for direct-download & archive sources |

- **Any agent:** these are standard `SKILL.md` skills — swap `-a opencode` for `claude-code`,
  `cursor`, `codex`, `windsurf`, `cline`, etc., list several (`-a opencode -a claude-code`), or
  use `-a '*'` for all detected agents — the CLI supports 70+ agents. Agents not yet in the list
  (e.g. Freebuff) can use the universal `.agents/skills/` folder (see Install above).
- **Updates take effect in new sessions** (no hot-reload).
- **Naming collisions:** skill names must be unique across global + project locations.
- Full upstream reference: [npm `skills`](https://www.npmjs.com/package/skills) ·
  [GitHub](https://github.com/vercel-labs/skills) · [skills.sh](https://skills.sh).
