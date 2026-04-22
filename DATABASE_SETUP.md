# Database Setup (Docker Compose)

This project uses SQL Server for local development and challenge evaluation.

## Why Docker Compose
- Reproducible setup for reviewers and interviewers.
- One standard command to start dependencies.
- Environment variables managed in one place.
- Easy reset/cleanup for fresh runs.

## Files
- docker-compose stack: [docker-compose.yml](docker-compose.yml)
- env template: [dev.env.example](dev.env.example)
- schema bootstrap script: [scripts/db-init.sh](scripts/db-init.sh)
- API dev connection string: [Development Project/Interview.Web/appsettings.Development.json](Development%20Project/Interview.Web/appsettings.Development.json)

## Prerequisites
1. Docker Desktop running.
2. .NET SDK installed.

## Quick Start
1. Create env file:
```bash
cp dev.env.example dev.env
```

2. Start SQL Server:
```bash
docker compose up -d
```

3. Apply schema:
```bash
chmod +x scripts/db-init.sh
./scripts/db-init.sh
```

4. Verify SQL container health:
```bash
docker compose ps
```

## Run API
```bash
dotnet run --project "Development Project/Interview.Web/Interview.Web.csproj"
```

API route example:
- GET http://localhost:4000/api/v1/products

## Reset Database (optional)
```bash
docker compose down -v
```
Then run Quick Start again.

## Notes
- This setup is for local development only.
- Credentials in `dev.env` are intentionally local-only and excluded from Git.
- In production, use secure secret management (not plaintext env files in repo).
