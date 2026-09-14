---
name: agents-md
description: "Create, complete, and audit a repo's AGENTS.md — the always-loaded AI operating file. Full anatomy (overview, reading protocol, commands, conventions, git rules, docs-sync, quality gates, shorthand triggers), deriving every section from the repo's real workflow instead of transplanting templates, and auditing an existing file for staleness against the current code. Use when onboarding a repo, writing or expanding an AGENTS.md, or after workflow changes that may have staled it."
metadata:
  internal: true
---

# AGENTS.md — the repo's AI operating file

`AGENTS.md` is the **always-loaded** instruction file that AI coding agents read at the
start of every session. Because it is loaded every time (unlike skills, which load
on-demand), it is the right home for anything the agent must know **before** any topic
comes up: the project map, hard rules, commands, and trigger phrases.

The craft: an AGENTS.md must describe the repo **as it actually is** — derived from the
real workflow, never transplanted from another repo or a generic template.

## When to use it

- Setting up an `AGENTS.md` for a repo that has none.
- Completing or expanding an existing one (a section is missing or thin).
- Auditing for staleness after the workflow changed (branch model, scripts, doc layout).
- The user references "agents instructions", "AI rules file", or similar.
- **Not for:** executing triggers defined in the file (follow them directly), or the
  trigger-table mechanics specifically (see `agents-shorthand` for that — this skill
  covers the table's place in the file; `agents-shorthand` owns its derivation).

## Project config (read per repo, derive — never transplant)

Everything in the file must come from the repo's real setup:

| What | Where to read it |
|---|---|
| Language/runtime, framework, package manager | manifest (`package.json`, `pyproject.toml`, …), lockfile |
| Build / dev / test / lint commands | manifest `scripts`, CI config, Makefile |
| Branch model (single protected default vs default+integration) | `git branch -vv`, remote HEAD |
| Remote name + push policy (which branches ever get pushed) | `git remote -v` + AGENTS.md/CI |
| Branch-naming + commit conventions | git history (`git log --oneline`) |
| Docs layout (`docs/`, root `*.md`, wiki) | repo tree |
| Quality gates (typecheck, lint, tests, custom check scripts) | scripts + CI |
| Deploy target/flow | CI, docs, hosting config |

## File anatomy (what a complete AGENTS.md contains)

Include a section only when the repo actually has that concern. Order matters — most
important for orientation first:

1. **What this repo is** — 2–4 lines: product/purpose, stack, the single most important
   architectural fact. The agent reads this before every task.
2. **Reading protocol / routing table** — "if you are going to touch X, read Y first."
   Map task domains to docs/files, so the agent loads the right context before acting.
   Include the action rule: read → act → update the doc if behavior changed.
3. **Commands** — the real commands: install, dev, build, test, lint/check, deploy.
   One table, with gotchas inline (slow cold start, required env, ordering).
4. **Conventions** — code style, file naming, component rules, "no X in Y" prohibitions.
   Only rules that are actually enforced or matter here; not generic style advice.
5. **Git rules** — branch model, branch naming, commit-message format, what never happens
   without the user asking (commit/push/merge/reset), conflict policy.
6. **Docs-sync rule** — code and docs must not drift: change code → update the matching
   doc in the same change. Name which files are synced to what.
7. **Quality gates / delivery gate** — what must pass before work is "done" (build,
   tests, checks, review steps).
8. **Shorthand trigger commands** — single-token `x!` phrases the user types **alone**.
   Own only the table here; derive rows per `agents-shorthand`.
9. **Context-gathering / scope rules** — how the agent should treat the file: read
   relevant docs before acting, docs are a knowledge base (flag conflicts, don't blindly
   execute), what "gather context" vs "take action" means if the repo defines it.

## Hard rules for writing the file

1. **Derive, never transplant.** Every section comes from the repo's real workflow. A
   section copied from a different repo can contradict this repo's rules (branch model,
   push policy, layout) — that is worse than a missing section.
2. **Placeholders over hardcoding.** Within any reusable fragment, use `{placeholders}`
   (`{Brand}`, `{default}`, `{remote}`) resolved from the project config — never bake in
   another project's names, paths, or IDs.
3. **Always-loaded means always-relevant.** Keep it lean; the file is re-read every
   session. Detailed procedures belong in skills/docs, referenced by pointer.
4. **One owner per rule.** A rule lives in exactly one section (or one doc it points to).
   Never state the same rule twice with different wording — drift starts there.
5. **Imperative, concrete, checkable.** "Run `npm run check:seo` before every build" —
   not "make sure things are good."
6. **Triggers stay here, not in skills.** A trigger typed alone has no topic for a skill
   description to match, so it must live in this always-loaded file.
7. **The file is a knowledge base, not an automation script.** Say so: when a doc
   conflicts with reality or the user's ask, the agent flags it — never mechanically
   executes an outdated instruction.
8. **Match the repo's existing tone/structure** if an AGENTS.md already exists — extend
   it, don't restyle it.

## Procedure — CREATE (no AGENTS.md)

1. Read the project config above until you can state the repo's workflow in one paragraph.
2. Draft the anatomy sections that apply, each derived from that workflow.
3. Show the draft to the user (section list + any decisions taken), get approval.
4. Write `AGENTS.md`, commit on a feature branch per the repo's git rules.

## Procedure — COMPLETE (exists, gaps)

1. Inventory the existing file against the anatomy: list what's present, missing, thin.
2. Derive the missing sections from the project config (same as CREATE).
3. Show the user the proposed additions; on approval, **append in the file's existing
   style** — do not restyle existing sections.

## Procedure — AUDIT (staleness check)

1. For each section, verify claims against the repo as it is now:
   - Commands still exist and work (check manifest scripts; run nothing destructive).
   - Paths/dirs in the routing table still exist; docs listed still exist.
   - Branch model, remote, push policy match git reality.
   - Quality gates reference scripts that still exist.
   - Trigger rows still describe workflows the repo actually uses.
2. Check the structural rules: no duplicated rules across sections; nothing hardcoded
   that should be a placeholder; no section so long it belongs in a skill/doc.
3. Propose fixes per section; apply approved edits.
4. Report: sections verified, sections fixed, sections removed (with why).

## Commands

```bash
cat AGENTS.md                       # the file under review
ls                                  # layout: docs/, src/, scripts/, …
cat package.json                    # or pyproject.toml / Makefile — real commands
git branch -vv && git remote -v     # branch model + push policy
git log --oneline -15               # commit-message + branch-naming conventions
```

## Checklist (done when)

- [ ] Every anatomy section that applies exists (and no dead sections for concerns the repo doesn't have).
- [ ] Each section was derived from the repo's real workflow, verifiable against manifest/git/docs.
- [ ] No rule is stated in two places with different wording.
- [ ] Commands section lists only commands that exist in the repo.
- [ ] Trigger table (if present) fires only when typed alone; record-only trigger documented as taking precedence.
- [ ] File states the knowledge-base rule (flag conflicts; never blindly execute).
- [ ] Audit mode: every section verified against the current repo; stale content fixed or removed with reasons.

## Notes & gotchas

- **AGENTS.md vs skills split:** the always-loaded file holds orientation + hard rules +
  triggers; on-demand skills hold detailed procedures. If a section grows past ~a screen,
  move the procedure into a skill and leave a pointer.
- **Don't duplicate the README.** README is for humans (setup, usage); AGENTS.md is for
  the agent (rules, workflows, gates). Overlap should be a pointer, not a copy.
- Nested agents: some tools read `AGENTS.md` from subdirectories for scoped rules — if
  the repo uses that, say so in the root file.
- Keep line length and formatting conventions consistent with the repo's existing docs.
- If the repo has no strong workflow yet, write a minimal file (overview + commands + git
  rules) and grow it as conventions solidify.
