# OrdreFlow Development Environment

This document describes the shared local development conventions for OrdreFlow.
Repository-specific commands remain in the README for the repository being
worked on.

## Supported Host Setups

The supported development setups are:

- Native Linux
- Windows with Ubuntu 24.04 running under WSL 2

Flox is installed and run inside Linux. Windows contributors should not expect
the Flox CLI to run as a native Windows tool. WSL 1 is not supported for this
workflow.

Repositories should be kept in the Linux filesystem inside WSL, such as under
`~/Projects`, rather than under `/mnt/c`. This keeps file access and tooling
behavior consistent with native Linux.

## Flox Repository Convention

Each code repository owns its Flox environment. A repository that uses Flox
commits its environment definition and lock file under `.flox/`.

The important files are:

- `.flox/env/manifest.toml` — declared packages and environment settings
- `.flox/env/manifest.lock` — resolved package versions and systems
- `.flox/env.json` — local environment metadata
- `global.json` — .NET SDK selection when the repository uses .NET

Flox runtime, cache, log, and telemetry files are local-only and are excluded
by the repository's Flox ignore rules. Manifest and lock-file changes should be
reviewed and committed together.

## Frontend Environment

The frontend uses a minimal Flox environment containing the exact .NET SDK
required by the current project:

- Target framework: `net8.0`
- SDK: `8.0.130`
- Flox package: `dotnetCorePackages.sdk_8_0_1xx-bin@8.0.130`

The frontend environment does not provide Node.js, PostgreSQL, Docker, Git, or
Entity Framework Core tools. PostgreSQL and Docker belong to backend or shared
service setup, and Git is expected to be installed outside the project Flox
environment.

## Frontend Setup

Install [Flox](https://flox.dev/docs/install-flox/install/) inside the Linux
environment before setting up the repository.

From the frontend repository root:

```bash
flox activate
```

Then run these commands inside the activated shell:

```bash
dotnet --version
dotnet restore
dotnet build
dotnet run --project src/OrdreFlow.Frontend
```

The version check should print `8.0.130`. The frontend README also provides a
single-command form using `flox activate -c`.

## Updating Packages

Run Flox package commands from the repository root:

```bash
flox search <package>
flox install <package>
```

Do not edit `manifest.lock` manually. Review changes to both
`.flox/env/manifest.toml` and `.flox/env/manifest.lock` before committing.

## Backend and Services

The backend-specific Flox environment has not been established yet. Its setup
must document the .NET SDK, PostgreSQL, Docker Compose, migrations, and any
backend-only tools when that repository is configured.

The frontend must not add backend services to its Flox manifest merely to make
the frontend build. Frontend and backend setup should remain independently
usable while following the same host and WSL conventions.

## Troubleshooting

If `dotnet --version` does not print `8.0.130`, activate the environment from
the frontend repository root and run the command again. The committed
`global.json` intentionally rejects a different SDK version.

If Flox cannot find the environment, confirm that the command is being run from
the repository root or pass the repository path with `-d`.

If using Windows, confirm that the command is running in Ubuntu under WSL 2,
not in native PowerShell or WSL 1.

## Related Documentation

- [Frontend README](https://github.com/ordreflow/ordreflow-frontend#running-locally)
- [Technology stack](technology-stack.md)
- [Branching and pull requests](branching-and-prs.md)
