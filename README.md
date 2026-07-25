# goodfood-ms-logs

Centralized logging microservice for the [GoodFood](https://github.com/RMurier/BAC-5-CUBE-1-COLLABORATIF) platform.

**Status:** 🚧 Scaffold — this is currently an unmodified `dotnet new webapi` template (only the sample `/weatherforecast` endpoint). No logging/ingestion logic has been written yet. See [`goodfood-ms-auth`](https://github.com/RMurier/goodfood-ms-auth#readme) for what a fleshed-out service in this platform looks like.

## Table of Contents

- [Intended Purpose](#intended-purpose)
- [Tech Stack](#tech-stack)
- [Environment Variables](#environment-variables)
- [Running Locally](#running-locally)
- [Tests](#tests)
- [CI/CD](#cicd)

## Intended Purpose

Collect and expose logs/events emitted by the other services for debugging and auditing — a shared observability sink rather than a customer-facing feature.

## Tech Stack

- .NET 9 / ASP.NET Core Web API
- SQL Server, via `Microsoft.EntityFrameworkCore.SqlServer` (not yet added — connection string is wired up, no `DbContext` exists yet)

## Environment Variables

| Variable | Description |
|----------|--------------|
| `ASPNETCORE_ENVIRONMENT` | `Development` or `Production` |
| `ConnectionStrings__DefaultConnection` | SQL Server connection string (database `GoodFood_Logs_Dev` in dev) |

## Running Locally

### Via the platform's docker-compose

From the [parent repo](https://github.com/RMurier/BAC-5-CUBE-1-COLLABORATIF):

```bash
docker compose -f docker-compose.dev.yml up -d db-sql-dev ms-logs-dev
```

Runs on `http://localhost:3008`, Swagger at `http://localhost:3008/swagger`.

### Standalone

```bash
cd GoodFood.Logs.Api
dotnet restore
dotnet run
```

## Tests

```bash
dotnet test GoodFood.Logs.Tests/GoodFood.Logs.Tests.csproj --verbosity normal
```

Currently a single sanity check ([`SanityTests.cs`](GoodFood.Logs.Tests/SanityTests.cs)), there to keep the CI test job green while the service is empty.

## CI/CD

Built, scanned (SonarQube, Trivy, OWASP Dependency-Check, GitGuardian) and published on every push, gated on all of them passing — see the [parent repo's CI/CD Pipeline section](https://github.com/RMurier/BAC-5-CUBE-1-COLLABORATIF#cicd-pipeline) for how the pipeline is wired across repos.
