---
name: <skill-name-lowercase-hyphenated>
description: "<One or two specific sentences: what this skill does AND when the agent should load it (the trigger). 1-1024 chars. Wrap in double quotes if it contains a colon-space like (foo: bar)."
metadata:
  internal: true
---

# <Human Readable Title>

<!--
HOW TO USE THIS TEMPLATE (delete these comments before committing):
1. Copy this file to  skills/<skill-name>/SKILL.md  — the folder name MUST equal `name` below.
2. `name`: lowercase letters/numbers + single hyphens, 1-64 chars, regex ^[a-z0-9]+(-[a-z0-9]+)*$
3. `description`: be SPECIFIC — the agent decides when to load the skill from this text alone.
   Say what it does + when to use it. Quote it if it contains ": ".
4. Keep the skill NICHE-AGNOSTIC: use {placeholders} and a "read the project config" step.
   Concrete examples are fine only as clearly-labeled illustrations.
5. Make the body self-contained and actionable — it should work without opening other files.
6. Validate:  INSTALL_INTERNAL_SKILLS=1 npx skills add . --list   (must appear, no YAML parse error)
7. Add a README table row + a CHANGELOG entry, then commit + push.
-->

## What this skill does

<1 short paragraph: the outcome this skill produces and who/when it's for.>

## When to use it

- <Trigger 1 — e.g. "the user asks to …">
- <Trigger 2>
- <Not-for: when NOT to use this skill>

## Project config (supply per use, never hardcode)

<For reusable skills: list the variables the caller must provide, e.g. {Brand}, {domain},
{region}, {location}, {service}, and where to read them (config file, docs, data).>

## Rules / steps

1. <Actionable step or hard rule.>
2. <…>
3. <…>

## Commands

```bash
# any commands the skill relies on, with the project's real values substituted
```

## Checklist (done when)

- [ ] <verifiable completion criterion>
- [ ] <another criterion>

## Notes & gotchas

- <Edge cases, common mistakes, constraints.>
