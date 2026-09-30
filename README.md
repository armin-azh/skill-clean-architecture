# Clean Architecture Project Skill

[![skills.sh](https://skills.sh/b/armin-azh/skill-clean-architecture)](https://skills.sh/armin-azh/skill-clean-architecture)

A reusable Agent Skill for starting and extending clean architecture projects. It helps an agent place domain rules, use cases, ports, adapters, configuration, migrations, and generated code in the right part of a repository.

The skill uses the KnowMe `core-app` Go service as a concrete example. Its rules work with other languages and technology choices: a project can use PostgreSQL or MySQL, sqlc or an ORM, Redis or another cache, or no database or cache at all.

## What it does

- Builds a project-specific architecture map before choosing paths.
- Starts new projects with the smallest useful vertical slice, without empty placeholder packages.
- Places new features along the domain, application, adapter, and composition boundaries.
- Distinguishes technologies that add files or processes from replacements behind an existing port.
- Records migration sources, handwritten queries, generated output, and the commands that keep them in sync.
- Preserves an existing project's conventions when extending it.

## Quick start

Keep this folder intact when publishing or installing the skill. If this folder is the root of a public GitHub repository, replace `armin-azh/skill-clean-architecture` below with its address:

```sh
npx skills add armin-azh/skill-clean-architecture --skill clean-architecture-project
```

For a local Codex installation, copy the complete `clean-architecture-project` folder into `~/.codex/skills/`. The `SKILL.md` and its `references/` directory must stay together.

Ask your agent to use the skill, for example:

> Use `clean-architecture-project` to initialize a Go HTTP service with MySQL migrations and no cache. Create one working use case and record the chosen paths.

> Use `clean-architecture-project` to add an order cancellation feature to this existing service. Follow its architecture map and current conventions.

> Use `clean-architecture-project` to replace the PostgreSQL adapter with MySQL. Identify which paths and generated artifacts change while keeping the application port stable where its contract allows it.

## Project-specific paths

The skill uses [`references/architecture-map.template.md`](references/architecture-map.template.md) to create `docs/architecture-map.md` in a target project. That map records where each responsibility belongs and which technologies have a physical footprint. An optional `AGENTS.md` instruction points future implementation work to the map.

## Contents

```text
SKILL.md
references/
  architecture-map.template.md
  core-app-example.md
README.md
```

Read [SKILL.md](SKILL.md) for the full workflow and dependency rules.


