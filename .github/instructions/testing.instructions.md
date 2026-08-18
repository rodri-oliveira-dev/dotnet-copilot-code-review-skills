---
applyTo: "**/*Tests/**/*.cs,**/*.Tests/**/*.cs,**/*Test.cs,**/*Tests.cs"
---

# .NET Test Review Guidelines

Apply these rules to changed test code.

## Test value

- Verify tests fail when the behavior under test is broken; avoid tautological tests and assertions that only restate mock setup.
- Prefer assertions on externally observable behavior rather than implementation details.
- Check important happy paths, boundaries, failures, authorization rules, concurrency cases, and regressions introduced by the pull request.
- Do not demand one test per method or arbitrary coverage percentages without repository policy or risk justification.

## Determinism and isolation

- Flag reliance on wall-clock time, random values, execution order, shared mutable state, machine-specific paths, external services, or global configuration when it makes tests flaky.
- Prefer controllable clocks/time providers for time-sensitive behavior when the target framework supports the project's chosen abstraction.
- Avoid `Thread.Sleep` as synchronization. Use deterministic coordination or await the observable condition.
- Ensure test-created resources are isolated and cleaned up.

## Async tests

- Await asynchronous operations and assertions.
- Flag fire-and-forget test execution, `async void`, blocking waits, and races caused by not awaiting background work.
- Propagate cancellation/timeouts intentionally in integration tests so a failed dependency does not hang the suite indefinitely.

## Mocks and doubles

- Mock external boundaries or expensive dependencies when appropriate, not the logic being tested.
- Flag excessive interaction assertions when they make harmless refactors break tests without protecting behavior.
- Verify configured mocks do not accidentally return impossible states that hide production bugs.

## Integration tests

- Check database, HTTP, queue, filesystem, and container-based tests for isolation and repeatability.
- Ensure tests use realistic serialization, routing, middleware, transaction, and persistence behavior when those concerns are the purpose of the test.
- Avoid replacing so much infrastructure that an "integration" test no longer exercises the integration risk.

## Do not report

- Arrange/Act/Assert comments as either required or forbidden unless the repository has an explicit convention.
- A specific test framework, mocking library, assertion library, or naming style purely by preference.
