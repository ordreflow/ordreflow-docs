# OrdreFlow Technology Stack

This document records the technology direction for the OrdreFlow proof of concept and core MVP. It distinguishes confirmed decisions from temporary choices and open decisions.

## Status Legend

- **Confirmed**: agreed by the project group
- **POC choice**: chosen to keep the first implementation simple
- **Future**: deliberately postponed
- **Open**: requires a later decision

## Architecture Overview

```text
Phone or desktop browser
        |
        | HTTP/JSON
        v
Blazor WebAssembly frontend
        |
        | REST API
        v
ASP.NET Core Web API backend
        |
        | EF Core + Npgsql
        v
PostgreSQL database
```

The frontend and backend are separate repositories. The frontend must not connect directly to PostgreSQL.

## Language and Runtime

### C# and .NET

Status: **Confirmed**

C# and .NET will be used across the application. The exact .NET version should be the current supported LTS version accepted by the development and hosting environments.

The frontend and backend currently target .NET 8 and pin SDK `8.0.130` through
their Flox environments and `global.json` files. This is the reproducible
development baseline for the current code projects; final hosting and
deployment compatibility remain open decisions.

See the [development environment documentation](development-environment.md)
for the host, WSL 2, and Flox conventions.

## Frontend

### Blazor WebAssembly

Status: **POC choice**

Blazor WebAssembly allows the browser-based frontend to be written primarily in C#. It supports multiple routes and components, so it is not limited to one screen. It is suitable for the phone-first employee flow and fits the separate frontend repository.

The frontend will still use Razor markup, HTML, and CSS. JavaScript interop may be needed for browser-specific capabilities.

### Responsive and Mobile-First UI

Status: **Confirmed**

The employee workflow must be designed for phones first because field workers use phones for the existing system. Administration and reporting can receive desktop-oriented layouts later.

The application should be tested on real phones during the POC.

### PWA and Offline Support

Status: **Future**

Blazor WebAssembly can be extended into a Progressive Web App. True offline time registration would require browser storage, queued operations, synchronization, conflict handling, and offline authentication behavior. These concerns are outside the first POC unless user research proves they are immediately required.

## Backend

### ASP.NET Core Web API

Status: **Confirmed**

The backend will expose a REST API using HTTP and JSON. It owns validation, authorization, business rules, and database access.

The API should expose DTOs rather than EF Core entities. The frontend should communicate through a typed client service rather than placing HTTP calls throughout UI components.

## Data Access

### Entity Framework Core

Status: **Confirmed**

EF Core will be used as the object-relational mapper. The backend domain model and `DbContext` will be used to create and evolve the database schema through migrations.

EF Core is not itself a database. It requires a database provider.

### Npgsql

Status: **Confirmed direction**

Npgsql is the EF Core provider used to connect the .NET backend to PostgreSQL.

### PostgreSQL

Status: **Confirmed**

PostgreSQL is the database system for the project. The development setup should use a real PostgreSQL instance, preferably through Docker, so local behavior is close to the intended deployed behavior.

Important data decisions for the POC:

- Store time duration as integer minutes rather than floating-point hours.
- Use a date-only value for the work date when a time of day is not required.
- Define a clear timezone policy before start/stop timing is implemented.
- Keep foreign keys between employees, orders, tasks, and time entries.
- Keep audit fields for important changes.

## Communication

### HTTP/REST and JSON

Status: **Confirmed direction**

The Blazor frontend will call the ASP.NET Core API using HTTP requests and JSON payloads.

HTTP is acceptable for local development. Test and production environments must use HTTPS because the application handles employee and business data.

The POC does not need GraphQL or SignalR. Those technologies can be considered later only if a concrete requirement appears.

## Authentication and Authorization

### Basic Authentication for Early Development

Status: **POC choice**

The POC may use local test users or a development-only authentication setup. The API must still make authorization decisions from the authenticated user and claims rather than trusting an employee ID supplied by the frontend.

### Microsoft Entra ID

Status: **Target solution**

Microsoft Entra ID, formerly Azure AD, is the intended authentication provider for a later stage. Authorization should use stable application roles such as `Employee`, `Leader`, and `Admin` so that the authentication provider can change without rewriting business rules.

## Testing

Status: **Confirmed direction**

The project should use:

- xUnit for backend unit and integration tests
- bUnit for Blazor component tests where useful
- Playwright for complete browser user flows
- Swagger or Postman for exploratory API testing

Manual Swagger or Postman testing is useful during development, but the core POC flow should also have repeatable automated tests.

## Hosting and Deployment

Status: **Open**

The final hosting environment has not been decided. It must eventually support:

- HTTPS access from field workers' phones
- API access from customer locations
- PostgreSQL connectivity
- Secret and connection-string configuration
- Backup and restore
- Deployment documentation

The final hosting decision can wait, but a reachable test deployment should be made during the POC.

## Open Technical Decisions

- Final .NET version supported by the company environment
- Hosting environment
- Local versus Entra ID authentication during the POC
- Manual duration versus start/stop time registration
- Same domain versus separate frontend and API domains
- CORS configuration for local and deployed environments
- Exact order, task, and time-entry data model
