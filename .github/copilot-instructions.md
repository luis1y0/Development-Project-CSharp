# Copilot Instructions For This Repository

## Project Context
This repository is a C# interview challenge to build an API-driven inventory system from an existing scaffold.

Core challenge file:
- [Development Project Specifications.txt](Development%20Project%20Specifications.txt)

Current repository state:
- Web API scaffold exists in [Development Project/Interview.Web](Development%20Project/Interview.Web)
- SQL schema exists in [Development Project/Sparcpoint.Inventory.Database](Development%20Project/Sparcpoint.Inventory.Database)
- Generic libraries exist in:
  - [Development Project/Sparcpoint.Core](Development%20Project/Sparcpoint.Core)
  - [Development Project/Sparcpoint.SqlServer.Abstractions](Development%20Project/Sparcpoint.SqlServer.Abstractions)

## Delivery Strategy (2-hour style)
Prioritize small, complete vertical slices with clean architecture over broad incomplete coverage.

Preferred requirement order:
1. Add Product (with metadata and categories)
2. Search Products (by category, metadata, and general details)
3. Inventory operations only if time remains

## Local Development Standards
Use Docker Compose for reproducible local database setup.

Setup docs and scripts:
- [DATABASE_SETUP.md](DATABASE_SETUP.md)
- [docker-compose.yml](docker-compose.yml)
- [dev.env.example](dev.env.example)
- [scripts/db-init.sh](scripts/db-init.sh)

Environment behavior:
- Keep secrets out of Git (`dev.env` ignored).
- Use `appsettings.Development.json` for local connection string.

## Architecture Guidelines
Keep separation of concerns explicit:
1. Controllers: transport only (HTTP routing, status codes, DTO mapping)
2. Services: business use-case orchestration
3. Repositories: SQL persistence details

Dependency direction:
- Controller -> Service interface
- Service -> Repository interface
- Repository -> SQL executor abstractions

Use transactions for multi-table writes.

## Coding Expectations
1. Prefer clear, readable code over clever abstractions.
2. Use DI and interfaces at boundaries.
3. Validate inputs and return meaningful status codes.
4. Keep endpoints async when I/O is involved.
5. Add brief `EVAL:` comments for interview-review-relevant decisions.

## Incremental Git Workflow
Use milestone-oriented commits.

Recommended commit sequence:
1. chore: local DB setup + docs
2. chore: API DI/config wiring
3. feat: add product endpoint
4. feat: search products endpoint
5. docs: final scope achieved and out-of-scope notes

Track progress in:
- [CHALLENGE_TRACKING.md](CHALLENGE_TRACKING.md)

## Review Checkpoints
Pause and request review at:
1. After environment + wiring
2. After first complete endpoint
3. Before final polish

## Known Practical Notes
1. SQL Server container on Apple Silicon may run under amd64 emulation; acceptable for challenge dev.
2. Port conflicts can happen locally; prefer dedicated API dev port in launch settings.
3. Keep setup commands reproducible in docs so reviewers can run quickly.
