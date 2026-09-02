# OrdreFlow Documentation

This repository contains project-wide documentation for OrdreFlow, the internal time-registration and order-management system being developed for VS Automatic.

## Repositories

- [Frontend](https://github.com/ordreflow/ordreflow-frontend)
- [Backend](https://github.com/ordreflow/ordreflow-backend)
- [GitHub Project](https://github.com/orgs/ordreflow/projects)

## Documentation Structure

- [`docs/technology-stack.md`](docs/technology-stack.md): current technology decisions, alternatives, and open decisions
- [`docs/task-planning-and-backlog.md`](docs/task-planning-and-backlog.md): issue creation, Project usage, backlog, and parent/child issue rules
- [`milestones/M0-inception-and-foundation.md`](milestones/M0-inception-and-foundation.md): project and technical foundation
- [`milestones/M1-proof-of-concept.md`](milestones/M1-proof-of-concept.md): first end-to-end working slice
- [`milestones/M2-core-employee-mvp.md`](milestones/M2-core-employee-mvp.md): core mobile employee functionality

Additional documentation such as the API contract, architecture diagram, domain glossary, branching strategy, and evaluation plan will be added as those decisions are made.

## Current Direction

The current direction is:

- Mobile-first web application
- Blazor WebAssembly frontend
- ASP.NET Core Web API backend
- HTTP/REST communication with JSON
- Entity Framework Core for data access
- PostgreSQL database
- Basic authentication for the early stages
- Microsoft Entra ID integration as the target authentication solution
- PWA and offline synchronization as future scope

## Documentation Principles

- GitHub Projects contains task status, assignments, and sprint planning.
- This repository contains project-wide decisions and milestone definitions.
- Each code repository contains its own setup and implementation-specific README.
- Documents should describe confirmed decisions separately from open decisions.
- Documentation should be updated when the implementation or project decision changes.

## Current Milestone

The current milestone is **M0: Inception and Foundation**.
