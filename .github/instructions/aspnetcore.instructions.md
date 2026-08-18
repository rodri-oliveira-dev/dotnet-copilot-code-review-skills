---
applyTo: "**/*.cs"
---

# ASP.NET Core Review Guidelines

Apply these checks when changed C# code participates in an ASP.NET Core application. Ignore framework-specific rules when the project is not an ASP.NET Core application.

## HTTP contracts

- Verify status codes and response bodies match the endpoint contract.
- Check accidental breaking changes to routes, parameter binding, request/response DTOs, serialization, content types, and public OpenAPI contracts.
- Check validation at trust boundaries. Do not duplicate domain validation merely to satisfy a checklist.
- Prefer standardized error responses such as Problem Details when that is already the project convention.
- Check cancellation propagation from `HttpContext.RequestAborted` or bound `CancellationToken` into I/O operations.

## Authentication and authorization

- Verify authentication is not confused with authorization.
- Check that sensitive endpoints require the intended policy, role, claim, resource authorization, or ownership check.
- Check authorization on every path that reaches protected data or operations, including alternate endpoints and background handoffs.
- Flag user-controlled identity or tenant identifiers when they can bypass server-side identity context.

## Dependency injection and lifetimes

- Check singleton dependencies for captured scoped/transient services and mutable shared state.
- Check expensive services or clients for inappropriate per-request construction.
- Prefer `IHttpClientFactory` or an intentional long-lived `HttpClient` strategy; do not flag every `HttpClient` construction without understanding ownership.
- Check hosted services for scope creation/disposal, shutdown behavior, retries, and cancellation.

## Middleware and pipeline

- Review middleware ordering when authentication, authorization, exception handling, routing, CORS, forwarded headers, rate limiting, caching, or static files are affected.
- Verify middleware calls the next delegate exactly when intended and does not accidentally swallow or duplicate requests.
- Check exception handlers for information disclosure and consistent production behavior.

## Configuration and secrets

- Do not allow secrets or credentials to be committed in source or application settings intended for source control.
- Check strongly typed options for validation when invalid configuration would fail later in production.
- Check environment-specific behavior for secure defaults.

## APIs and operations

- Check idempotency when retryable write endpoints can duplicate externally visible effects.
- Check pagination or bounded responses when endpoints can return unbounded collections.
- Check uploaded files, URLs, paths, headers, and forwarded values as untrusted input.
- Check health/readiness probes for dependency semantics; liveness should not normally fail because an external dependency is unavailable.

## Do not report

- Controller versus Minimal API as a preference.
- MediatR, endpoint libraries, CQRS, vertical slices, Clean Architecture, or any routing abstraction unless the repository already requires it or the current design causes a concrete problem.
