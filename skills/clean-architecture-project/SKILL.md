---
name: clean-architecture-project
description: Initialize or extend a clean architecture codebase with clear placement for domain, use cases, ports, adapters, configuration, migrations, and generated code. Use when starting a project, adding a feature or integration, or changing a technology that may affect its folder layout.
---

# Clean architecture project

Use architectural responsibilities to decide where code belongs. Treat `core-app` as an example of one Go layout, not as a mandatory technology stack. Preserve the target project's language conventions, existing paths, and explicit instructions.

## Start with the project map

1. Read the target repository's `AGENTS.md` and architecture documentation. If `docs/architecture-map.md` exists, use it as the placement map. Inspect representative files to confirm it still matches the code.
2. If the map is missing, identify the current entrypoints, domain model, use cases, inbound adapters, outbound adapters, configuration, schema changes, generators, and tests. For an established project, document the paths it already uses. For a new project, start from [the architecture map template](references/architecture-map.template.md), adapting names to the language and runtime.
3. Record material technology choices in the map: persistence engine and migration tool, query generator or ORM, cache, messaging, transport, and any generated output. Mark unused components as absent. Do not create directories for absent components.
4. If a convention is undecided and blocks placement, ask for that decision. Otherwise choose a coherent convention and record it before implementing dependent files.

The project map is the portable hook. It can live in any repository with this skill installed. Optionally add this instruction to that repository's `AGENTS.md`: “For project initialization and feature work, use the `clean-architecture-project` skill and follow `docs/architecture-map.md`; update the map when paths or technology footprints change.”

## Boundaries

- **Domain:** entities, value objects, and business invariants. It does not import delivery frameworks, database clients, cache clients, or generated transport and persistence models.
- **Application:** use cases and ports required by those use cases. It coordinates domain behavior and defines needed transaction or consistency guarantees. Ports belong with their consumer, not with a concrete adapter.
- **Inbound adapters:** HTTP, CLI, event consumers, workers, or other entry mechanisms. They decode and validate external shapes, invoke use cases, and map results and errors.
- **Outbound adapters:** persistence, cache, broker, external API, files, and similar implementations of application ports. Keep technology types and generated database models here; map them to application and domain types at the boundary.
- **Composition roots:** one per executable or deployable process as needed. They load validated configuration, connect adapters to ports, and own startup and shutdown.
- **Configuration:** parse external settings into typed, validated values at the outer boundary. Keep secrets and environment reads out of domain and application code.

Dependencies point inward. Keep framework, driver, and generator dependencies out of the domain and use cases. A folder name alone does not establish a boundary; check imports and data flow.

## Decide whether a technology changes the structure

Classify by its *actual footprint in this project*, not by product name:

| Change | Placement decision |
| --- | --- |
| Adds a new entrypoint, process, external capability, schema workflow, or generated artifact | Add only the required adapter, composition, configuration, schema, or generation paths; update the map. |
| Replaces a tool behind an existing port without changing responsibilities | Keep the architectural layers and port stable; change the adapter, composition, configuration, and verification as needed. |
| Changes dialect, generator, source format, or output path | Update the affected migration/query/generated paths and commands in the map; regenerate from source. |

For example, PostgreSQL to MySQL usually keeps the repository port and use case intact, but changes the driver adapter, SQL dialect, migration compatibility, generator configuration, and perhaps generated output. Adding Redis for caching creates an outbound adapter and wiring only if a use case actually needs a cache port; it does not create a new architectural layer. A logging library swap usually stays within outer setup and adapters. Reassess these examples against the target repository rather than copying their paths.

## Initialization

For a new project, establish the smallest working vertical slice:

1. Choose language, runtime, first use case, entrypoint, and required persistence or external capabilities.
2. Create the mapped domain, application, inbound adapter, outbound adapter, configuration, and composition paths that this slice needs. Put unit tests beside or near the code according to language conventions.
3. If schema migrations are used, choose one migration directory and naming scheme. If a query generator is used, separate its handwritten schema/query sources from generated output and configure the output inside the persistence adapter. If neither is used, omit both.
4. Add repeatable build, test, migration, and generation commands for tools actually present. Document how to run the slice.
5. Verify dependency direction and run the relevant format, generation, build, and test commands.

The template offers a `core-app` style Go starting layout and a role map for other languages. It is a starting point, not a request to scaffold every optional directory.

## Feature and technology changes

Trace the requested behavior from entrypoint to use case to domain and required ports. Place each change in its mapped layer. Add concrete adapters and wire them at the composition root. Keep transport, generated SQL, and vendor types at their boundaries. When a schema or generator source changes, generate output with the project's command; do not edit generated files by hand. Update the map when any path, process, generator input/output, or adapter responsibility changes.

Verify the smallest relevant behavior, plus real database/cache/broker semantics when correctness depends on them. Report the files placed, the boundary decisions, commands run, and any unresolved technology choice.

## Reference

- [Architecture map template](references/architecture-map.template.md): copy and adapt during initialization, or use it to document an established project.