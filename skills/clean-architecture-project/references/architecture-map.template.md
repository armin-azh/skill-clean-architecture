# Project architecture map template

Copy this file to `docs/architecture-map.md` in the target project and replace the bracketed values. Remove unused rows. Keep paths relative to that project's root.

## Decisions

| Decision | Project choice |
| --- | --- |
| Language and module/package root | [choice] |
| Executables or deployable processes | [names and entrypoints] |
| First or current use cases | [features] |
| Inbound protocols | [HTTP, CLI, events, etc.] |
| Persistence engine and adapter | [engine, driver/ORM, path, or absent] |
| Migration tool and source path | [tool, path, naming rule, or absent] |
| Query/code generator | [tool, source paths, output path, command, or absent] |
| Cache | [provider, adapter path, or absent] |
| Other external systems | [broker/API/files and adapter paths, or absent] |
| Test and verification commands | [commands] |

## Placement

| Responsibility | Path in this project | Allowed dependencies |
| --- | --- | --- |
| Domain types and rules | [path] | [language standard library and approved pure dependencies] |
| Use cases and consumed ports | [path] | [domain and pure dependencies] |
| Inbound adapters | [path per protocol] | [application and protocol dependencies] |
| Outbound adapters | [path per capability] | [application ports, domain mapping, vendor clients] |
| Composition roots | [path per process] | [all required outer modules] |
| Configuration | [path] | [configuration libraries, environment] |
| Migration sources | [path or absent] | [database dialect and migration tool] |
| Handwritten query sources | [path or absent] | [schema and query generator] |
| Generated output | [path or absent] | [generated; never hand-edit] |
| Tests | [path convention] | [test framework and fakes/real dependencies as needed] |

## Starter layout for a new Go service

Use this only if the project has no established layout. Create directories when the first feature needs them. Rename paths for another language while keeping the responsibilities and dependency direction.

```text
src/cmd/<process>/main.go                     # composition root
src/internal/domain/<feature>/                # business model and rules
src/internal/application/<feature>/           # use case and consumed ports
src/internal/transport/<protocol>/<feature>/ # inbound adapter
src/internal/infrastructure/<capability>/     # outbound adapter
src/internal/config/                          # typed configuration
migrations/                                   # optional versioned schema sources
```

If using SQL generation, record the exact handwritten schema and query locations, config file, output package, and generation command above. Keep the generated package within the database adapter. If replacing a database engine, review SQL dialect, migration history, driver wiring, and generator settings; do not change use case ports unless their contract truly changes.

## Integration hook

If the target repository uses `AGENTS.md`, this optional sentence keeps the map discoverable:

> For project initialization and feature work, use the `clean-architecture-project` skill and follow `docs/architecture-map.md`; update the map when paths or technology footprints change.