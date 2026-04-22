# Engineering Notes: .NET Jargon and Architecture Concepts

This document explains common .NET terms you will see in this challenge, plus architecture/design concepts that apply across stacks.

## 1) .NET Jargon Quick Guide

### Runtime
- .NET runtime is the execution engine for your app.
- Stack-agnostic equivalent: virtual machine/runtime layer (like JVM, Node runtime).

### SDK vs Runtime
- SDK is for building (compile, test, tooling).
- Runtime is for executing compiled apps.
- Equivalent: language toolchain vs production runtime image.

### Project (`.csproj`) and Solution (`.sln`)
- `.csproj`: one buildable unit (app or library).
- `.sln`: groups multiple projects.
- Equivalent: package/module + workspace/monorepo manifest.

### ASP.NET Core Controller
- Class that handles HTTP requests and returns HTTP responses.
- Equivalent: route handler/controller in Express, Laravel, Spring, Django.

### `IActionResult`
- Abstraction for HTTP responses (`200`, `404`, `400`, etc.).
- Equivalent: framework response wrapper type.

### Dependency Injection (DI)
- Register dependencies once, framework injects them into constructors.
- Equivalent: IoC containers used in many ecosystems.

### Middleware / Pipeline
- Ordered request-processing components.
- Equivalent: middleware chain in Express/Koa/Laravel/Spring filters.

### `appsettings.json` + environments
- Config files loaded by environment (Development, Production).
- Equivalent: environment-specific config files + env vars.

### DTO (Data Transfer Object)
- Request/response shape for API boundaries.
- Equivalent: transport schema object.

### Repository and Service
- Repository: persistence-focused data access.
- Service: business rules/orchestration.
- Equivalent: persistence adapter + use-case/application layer.

### Asynchronous APIs (`Task`, `Task<T>`)
- Promise-like async return types for non-blocking I/O.
- Equivalent: Promise/async-await in JS, futures in other languages.

## 2) Architecture Concepts (Framework-Agnostic)

### Separation of Concerns
Keep responsibilities isolated:
- Web/API layer: transport concerns (HTTP, status codes, DTO mapping)
- Application layer: use cases/business rules
- Infrastructure layer: database and external systems

Why it helps:
- Easier testing
- Easier refactoring
- Lower coupling to frameworks

### Dependency Inversion
High-level business logic should depend on interfaces, not concrete DB/framework code.

Practical rule:
- Controller depends on `IProductService`
- Service depends on `IProductRepository`
- Repository depends on SQL abstractions

### Transaction Boundary
Any multi-table write should be atomic.

Practical rule:
- Add product + metadata + category links in one transaction.

### Vertical Slice Delivery
Implement a thin end-to-end feature path first.

Practical rule for this challenge:
- Start with one write endpoint and one read endpoint fully working.

### Progressive Hardening
Start simple, then tighten with:
- Validation
- Error handling
- Logging
- Tests

## 3) Why This Challenge Starts with Generic Code

The provided solution is intentionally scaffold-heavy:
- Generic abstractions/libraries are reusable foundation
- Database schema captures the domain intent
- API layer is mostly placeholder for candidate implementation

This is common in interviews because it evaluates:
- ability to navigate an existing codebase
- quality of incremental design decisions
- practical delivery under time constraints

## 4) Suggested Design Decisions for This Challenge

### In scope for 2 hours
- Implement two high-ROI requirements well:
  - Add Product
  - Search Products

### Out of scope unless extra time
- Full inventory transaction lifecycle (add/remove/undo)
- Advanced querying and pagination optimizations
- Extensive test suite

### Implementation principles
- Keep request/response contracts explicit
- Return meaningful status codes
- Avoid over-generalization in first pass
- Prefer clear SQL over clever SQL
- Make commit history explainable milestone by milestone

## 5) Code Review Checklist (Stack-Agnostic)

- Is each endpoint tied to one clear use case?
- Are validation and error paths explicit?
- Are transactions used where needed?
- Is business logic outside controllers?
- Are interfaces used at layer boundaries?
- Are names meaningful and consistent?
- Can a new engineer find flow quickly?

## 6) Personal Learning Map (Next Steps)

1. Build first vertical slice (create product).
2. Add one search path.
3. Add one inventory operation.
4. Introduce automated tests for critical paths.
5. Refactor only after behavior is stable.

---

If you want, this file can be expanded into a bilingual (English/Spanish) quick-reference before submission.
