---
applyTo: "**/*.cs"
---

# C# Review Guidelines

Apply these rules when reviewing C# source files.

## Correctness and language semantics

- Check nullable reference type contracts and flag only real nullability risks. Trust non-null annotations unless control flow or external input makes the contract unsafe.
- Check equality, comparison, culture, casing, and string operations when they can change behavior across cultures or protocols.
- Check exception handling for swallowed exceptions, overly broad catches, lost stack traces, incorrect retry behavior, and exceptions used for normal control flow in hot paths.
- Check `IDisposable` and `IAsyncDisposable` ownership. Resources created by the changed code must have a clear lifetime and disposal path.
- Check iterators, deferred LINQ execution, closures, captures, and multiple enumeration when they can change correctness or materially increase cost.
- Check mutable shared state, static state, and caches for thread-safety assumptions.

## Async code

- Prefer async all the way for I/O-bound operations.
- Flag `.Result`, `.Wait()`, `GetAwaiter().GetResult()`, synchronous blocking over asynchronous APIs, and unnecessary `Task.Run` when they introduce deadlock, starvation, or scalability risk.
- Verify `CancellationToken` is propagated through meaningful asynchronous boundaries when cancellation is part of the caller contract.
- Do not recommend `ConfigureAwait(false)` for normal ASP.NET Core application code as a blanket rule.
- Do not recommend `ValueTask` without evidence that the API is on a hot path and frequently completes synchronously.
- Flag `async void` except for event handlers or framework-required signatures.
- Verify fire-and-forget work has explicit lifetime, error handling, and shutdown semantics.

## Style and modern C#

- Follow `.editorconfig`, analyzers, and nearby repository conventions before suggesting style changes.
- Do not require the newest C# syntax if the repository does not target that language version.
- Prefer clarity over novelty. Pattern matching, primary constructors, collection expressions, records, spans, and other modern features are suggestions only when they simplify code without changing its supported runtime/language constraints.
- Do not require XML documentation on every public member unless the repository or package contract expects it.

## API design

- For public APIs, review source and binary compatibility, nullability annotations, exception contracts, cancellation, and ownership semantics.
- Avoid exposing mutable collections when mutation is not part of the contract.
- Flag ambiguous boolean parameters, surprising side effects, or inconsistent naming only when they make misuse likely.

## Do not report

- Cosmetic brace, namespace, `var`, expression-bodied-member, or naming preferences that conflict with or duplicate repository formatting rules.
- "Make this method smaller" without identifying a concrete readability, testing, correctness, or reuse problem.
- "Apply SOLID" without naming the violated responsibility or dependency problem and its impact.
