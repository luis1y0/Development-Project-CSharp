# C# Inventory Challenge Tracking

## Objective
Deliver the highest-ROI requirements within a 2-hour window with clean architecture and incremental commits.

Selected scope for the timebox:
- Add Product (with metadata and categories)
- Search Products (by general details, metadata, and category)
- Inventory add/remove/count on individual products

Deferred unless extra time:
- Undo transaction endpoint
- Inventory count by metadata subset
- Automated tests beyond minimal smoke checks

## Timebox
- Start: 2026-04-17
- End: 2026-04-17 (+2h target)
- Elapsed: in progress
- Remaining: in progress

## Milestones

### M0 - Environment and Database Ready
- Status: done
- Goal: Confirm API run baseline, avoid local port conflicts, start SQL Server, deploy schema.
- Deliverables:
  - API listens on stable local URL
  - SQL Server available locally (Docker recommended)
  - Database schema deployed from provided scripts
- Commit target: `chore/setup-local-db-and-config`
- Outcome:
  - SQL Server container `inventory-sql` is running on port 1433
  - Database `inventory` created
  - Schema scripts from `Sparcpoint.Inventory.Database.sqlproj` executed in project-defined order
  - Verified tables:
    - `Instances.Products`
    - `Instances.ProductAttributes`
    - `Instances.ProductCategories`
    - `Instances.Categories`
    - `Instances.CategoryAttributes`
    - `Instances.CategoryCategories`
    - `Transactions.InventoryTransactions`
  - Local setup documentation created in `DATABASE_SETUP.md`
  - Docker Compose workflow added via `docker-compose.yml` + `dev.env.example`

### M1 - Wire API Infrastructure
- Status: done
- Goal: Connect Web API to database abstractions via DI and configuration.
- Deliverables:
  - Connection string in Development settings (done)
  - Project references added to Web project (done)
  - Service + repository interfaces and registrations (next)
- Commit target: `chore/wire-di-and-db-settings`

### M2 - Requirement 1: Add Product
- Status: done
- Goal: Implement `POST /api/v1/products` end-to-end.
- Deliverables:
  - Request/response DTOs
  - Transactional insert in Products, ProductAttributes, ProductCategories
  - Validation and clear status codes
- Commit target: `feat/products-create-endpoint`

### M3 - Requirement 2: Search Products
- Status: done
- Goal: Implement `GET /api/v1/products/search` with optional filters.
- Deliverables:
  - Search by name/details
  - Search by metadata key/value
  - Search by category
- Commit target: `feat/products-search-endpoint`

### M4 - Final Review and Submission Notes
- Status: done
- Goal: Final build/run verification and concise summary of delivered scope.
- Deliverables:
  - Build succeeds
  - Endpoints manually verified
  - Scope achieved vs out-of-scope documented
- Commit target: `docs/final-challenge-summary`
- Outcome:
  - Build verified successfully (`dotnet build`)
  - Manual endpoint checks re-verified on 2026-04-18:
    - `POST /api/v1/products` -> `201`
    - Validation failure on empty name -> `400`
    - `GET /api/v1/products/search?q=Trail` -> `200`
    - `POST /api/v1/products/{id}/inventory/add` -> `200`
    - `GET /api/v1/products/{id}/inventory/count` -> `200`

### M5 - Inventory Operations (Lean Slice)
- Status: done
- Goal: Implement inventory add/remove and count for a single product.
- Deliverables:
  - `POST /api/v1/products/{productId}/inventory/add`
  - `POST /api/v1/products/{productId}/inventory/remove`
  - `GET /api/v1/products/{productId}/inventory/count`
  - Validation for `productId` and `quantity`
- Commit target: `feat/inventory-operations-endpoints`

## Validation Log
| Timestamp | Command / Action | Outcome | Evidence |
|---|---|---|---|
| 2026-04-17 | `dotnet build Interview.Web.csproj` | Success | Local terminal |
| 2026-04-17 | `dotnet run ...Interview.Web.csproj --no-build` | Success | API listened on localhost |
| 2026-04-17 | `GET /api/v1/products` | Success (200) | Returned placeholder payload |
| 2026-04-17 | Docker SQL Server setup | Success | Container `inventory-sql` ready on 1433 |
| 2026-04-17 | Schema deployment to `inventory` DB | Success | All sqlproj build scripts executed |
| 2026-04-17 | Table verification query | Success | Instances + Transactions tables found |
| 2026-04-17 | `dotnet build Interview.Web.csproj` after M1 wiring | Success with warnings | 0 errors, 8 obsolete SqlClient warnings |
| 2026-04-17 | `docker compose config` | Success | Compose file validated |
| 2026-04-17 | `bash -n scripts/db-init.sh` | Success | Bootstrap script syntax valid |
| 2026-04-17 | Product create smoke tests | Success | POST create + validation checks passed |
| 2026-04-17 | Product search smoke tests | Success | GET search queries returned expected payloads |
| 2026-04-17 | Inventory smoke tests | Success | add/remove/count curl tests passed |
| 2026-04-17 | Commit `a9b5399` | Success | Create + search endpoints committed |
| 2026-04-17 | Commit `58b7fa1` | Success | Search hydration optimization committed |
| 2026-04-17 | Commit `52b242a` | Success | Inventory endpoints committed |
| 2026-04-18 | Final M4 verification (`dotnet build` + product/search/inventory curls) | Success | Build succeeded; statuses `201/400/200/200/200` with expected response payloads |

## Working Agreement (Cross-Chat)
- Until final packaging, commit only code files related to challenge implementation.
- Keep documentation files local and out of commits (`*.md`, planning notes, and setup notes) unless explicitly requested.
- Final docs/readme selection will be decided at the end of implementation.

## Risks and Mitigations
- Risk: Port conflicts on local machine.
  - Mitigation: Use a non-conflicting app URL in launch settings.
- Risk: Runtime mismatch for app execution (`net8.0` app with only .NET 10 runtime installed).
  - Mitigation: Install .NET 8 runtime/SDK or retarget app framework (retargeting is lower priority for challenge).
- Risk: SQL Server setup consumes too much time.
  - Mitigation: Use Docker first; if blocked, switch to managed SQL quickly.
- Risk: Over-scoping feature set.
  - Mitigation: Lock scope to M2 + M3 before any extras.

## Questions for Reviewer
- Preferred first delivery focus if time gets tight:
  - A) Add Product only (deeper quality)
  - B) Add Product + Search (broader coverage)

## Ready-for-Review Checkpoints
- Review #1: After M1 (infrastructure wiring)
- Review #2: After M2 (first complete requirement)
- Review #3: Final M4 pass

## Current Ask to Reviewer
- Confirm preferred delivery scope if time becomes tight:
  - Option A: complete `Add Product` only with strong quality and validation
  - Option B: complete `Add Product` + `Search Products` with leaner depth
- Confirm runtime approach:
  - Option A: install .NET 8 runtime/SDK locally
  - Option B: keep current SDK and retarget project to `net10.0` for local execution
