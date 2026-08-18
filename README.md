<p align="center">
  <strong>English</strong> · <a href="./README.pt-BR.md">Português (Brasil)</a>
</p>

# .NET Copilot Code Review Skills

Reusable GitHub Copilot instructions and agent skills for reviewing .NET and C# pull requests with a strong focus on correctness, security, performance, async/concurrency, persistence, testing, APIs, architecture, MSBuild, and NuGet.

The goal is to make Copilot Code Review behave more like an experienced .NET reviewer and less like a style linter: findings should be high-confidence, tied to concrete impact, and limited to changes introduced or materially affected by the pull request.

> Copilot instructions for this language: [`.github/copilot-instructions.md`](./.github/copilot-instructions.md)

## What this repository provides

This repository separates always-on guidance from task-specific review knowledge:

- `.github/copilot-instructions.md` contains repository-wide review behavior.
- `.github/instructions/*.instructions.md` contains path-specific rules for C#, ASP.NET Core, tests, persistence, and the .NET project system.
- `.github/skills/code-review/SKILL.md` orchestrates the review workflow.
- `.github/skills/code-review/references/*.md` contains focused checklists that the skill can load only when relevant.

This structure follows GitHub's current Copilot Code Review customization model: repository-wide instructions for general rules, path-specific instructions for matching files, and agent skills under `.github/skills` for task-specific workflows.

## Installation

Copy the `.github` directory from this repository into the root of the .NET repository you want Copilot to review:

```text
<your-repository>/
└── .github/
    ├── copilot-instructions.md
    ├── instructions/
    └── skills/
```

Commit these files to the branch being reviewed. GitHub Copilot Code Review reads repository instructions and skills from the pull request's **head branch**, so you can test changes to the review rules in the same pull request that introduces them.

Then request **Copilot** as a reviewer on a pull request, or configure automatic Copilot reviews in the repository settings.

### Using the Portuguese instructions

GitHub Copilot automatically recognizes the canonical `.github/copilot-instructions.md` path. The Portuguese translation is stored as `.github/copilot-instructions.pt-BR.md` so both language versions can coexist in this repository.

To use the Portuguese instructions in a target repository, copy `.github/copilot-instructions.pt-BR.md` and save it there as `.github/copilot-instructions.md`.

## Repository structure

```text
.github/
├── copilot-instructions.md
├── copilot-instructions.pt-BR.md
├── instructions/
│   ├── aspnetcore.instructions.md
│   ├── csharp.instructions.md
│   ├── persistence.instructions.md
│   ├── project-system.instructions.md
│   └── testing.instructions.md
└── skills/
    └── code-review/
        ├── SKILL.md
        └── references/
            ├── architecture.md
            ├── async-concurrency.md
            ├── dapper.md
            ├── dotnet-performance.md
            ├── ef-core.md
            ├── msbuild.md
            ├── security.md
            ├── testing.md
            └── webapi.md
```

## What each file does

| File | Purpose |
| --- | --- |
| `.github/copilot-instructions.md` | Defines the global review contract: diff-first analysis, high-confidence findings, severity levels, evidence requirements, and noise-reduction rules. |
| `.github/copilot-instructions.pt-BR.md` | Portuguese (Brazil) translation of the global review contract. Rename/copy it to the canonical `copilot-instructions.md` path when Portuguese instructions should be active. |
| `instructions/csharp.instructions.md` | Reviews C# language usage, nullability, exceptions, resource disposal, cancellation, naming, and modern language features without forcing stylistic preferences. |
| `instructions/aspnetcore.instructions.md` | Reviews ASP.NET Core endpoints, middleware, DI lifetimes, authentication/authorization, validation, Problem Details, configuration, health checks, and HTTP semantics. |
| `instructions/testing.instructions.md` | Reviews unit/integration tests for meaningful assertions, determinism, isolation, edge cases, async correctness, and excessive mocking. |
| `instructions/persistence.instructions.md` | Applies persistence-wide rules to EF Core, Dapper, ADO.NET, SQL, transactions, parameterization, query shape, and cancellation. |
| `instructions/project-system.instructions.md` | Applies targeted rules to `.csproj`, `.props`, `.targets`, `Directory.Build.*`, Central Package Management, `global.json`, solution files, packaging, and publish settings. |
| `skills/code-review/SKILL.md` | Main review orchestrator. Determines which specialist references are relevant, prioritizes findings, and defines the expected review output. |
| `references/architecture.md` | Checks dependency direction, boundaries, coupling, cohesion, API contracts, abstractions, and architectural consistency. It explicitly avoids recommending patterns without a concrete problem. |
| `references/async-concurrency.md` | Checks sync-over-async, blocking calls, fire-and-forget work, cancellation propagation, race conditions, locks, shared state, `Task`/`ValueTask`, and async disposal. |
| `references/dapper.md` | Checks parameterized SQL, connection/transaction ownership, buffering, multi-mapping, command timeouts, cancellation, result cardinality, and common Dapper pitfalls. |
| `references/dotnet-performance.md` | Checks allocations, LINQ/string/collection usage, regex, serialization, I/O, pooling, hot-path concerns, and premature optimization risks. |
| `references/ef-core.md` | Checks N+1 queries, tracking, projections, `Include`, split queries, pagination, query translation, indexes, bulk operations, transactions, and concurrency. |
| `references/msbuild.md` | Checks target frameworks, SDK/language compatibility, package references, MSBuild conditions/imports/targets, Central Package Management, publish behavior, and NuGet packaging. |
| `references/security.md` | Checks authentication, authorization, injection, secrets, sensitive logging, SSRF/path traversal, unsafe deserialization, cryptography, and secure configuration. |
| `references/testing.md` | Provides the deeper test-review checklist used by the skill when test files or behavior changes are present. |
| `references/webapi.md` | Checks API contracts, status codes, validation, cancellation, idempotency, pagination, versioning, Problem Details, HTTP semantics, and Minimal API/controller concerns. |

## How the review works

The skill uses a **diff-first** workflow:

1. Understand the pull request intent and changed behavior.
2. Inspect the diff before judging surrounding code.
3. Run the core correctness/security/compatibility checks.
4. Detect relevant technical signals in the changed files.
5. Load only the specialist references that match those signals.
6. Validate each candidate finding before reporting it.
7. Prefer zero findings over speculative feedback.

Examples:

- A pull request changing `DbContext` queries loads the EF Core reference.
- A Dapper repository change loads the Dapper and persistence guidance.
- A `BackgroundService` change loads async/concurrency guidance.
- A controller or Minimal API change loads Web API guidance and, when applicable, security guidance.
- A `.csproj` or `Directory.Packages.props` change loads the MSBuild/NuGet reference.
- A simple domain class change does **not** automatically load every specialist checklist.

## Review severity

The skill uses four levels:

- **BLOCKER** — exploitable security issues, data loss/corruption, severe correctness defects, deadlocks, or incompatible breaking changes that make merging unsafe.
- **HIGH** — likely production defects, authorization mistakes, serious concurrency/resource issues, major regressions, or build/package changes that break supported consumers or deployment targets.
- **MEDIUM** — maintainability, performance, testability, architectural, or project-system problems with concrete impact but that do not necessarily block the merge.
- **LOW** — useful, evidence-backed improvements that are genuinely relevant to the changed code. Style-only preferences should not be reported.

## Review philosophy

The reviewer should:

1. Review the pull request diff before judging the surrounding codebase.
2. Report only issues with a plausible failure mode or measurable engineering cost.
3. Explain why each finding matters.
4. Prefer repository conventions and `.editorconfig` over personal style preferences.
5. Avoid commenting on unchanged legacy code unless the pull request introduces, exposes, or materially worsens the problem.
6. Avoid recommending Repository, CQRS, MediatR, Clean Architecture, DDD, factories, interfaces, or any other pattern merely because the pattern exists.
7. Treat performance findings as context-dependent and avoid micro-optimization outside hot paths.
8. Avoid duplicate comments when multiple lines share the same root cause.
9. Prefer a small number of actionable findings over a large number of speculative observations.
10. Respect the repository's actual `TargetFramework`, `LangVersion`, package-management model, and build conventions rather than requiring the newest .NET/C# feature.

## Customizing for a repository

These files are intentionally architecture-neutral. Add project-specific constraints to the target repository rather than changing the generic rules globally.

Examples of useful project-specific additions:

- supported .NET/C# versions;
- architectural boundaries that actually exist in the project;
- domain invariants;
- API compatibility requirements;
- required observability fields;
- database-specific constraints;
- expected test frameworks and conventions;
- performance-sensitive paths;
- security or compliance requirements;
- NuGet/package compatibility requirements;
- deployment/publish constraints.

For example, if a project deliberately uses Hexagonal Architecture, document its actual dependency rules in that project's `.github/copilot-instructions.md`. Do not make Hexagonal Architecture a universal requirement for every .NET repository.

## Sources and inspiration

The rules in this repository are consolidated and adapted rather than copied verbatim. Important references include:

- [GitHub Docs — About Copilot code review](https://docs.github.com/en/copilot/concepts/agents/code-review)
- [GitHub Docs — Customize Copilot code review](https://docs.github.com/en/copilot/tutorials/customize-code-review)
- [github/awesome-copilot](https://github.com/github/awesome-copilot)
- [dotnet/skills](https://github.com/dotnet/skills)
- [dotnet/skills — .NET performance analysis](https://github.com/dotnet/skills/tree/main/plugins/dotnet-diag/skills/analyzing-dotnet-performance)
- [dotnet/skills — MSBuild code review agent](https://github.com/dotnet/skills/blob/main/plugins/dotnet-msbuild/agents/msbuild-code-review.agent.md)

The .NET performance material in `dotnet/skills` is MIT-licensed. Review the licenses of upstream projects before directly copying upstream content into derivative distributions.

## Scope

This project is aimed primarily at modern .NET applications and libraries. The guidance is useful for .NET 8+ codebases, but most rules are version-independent. Version-specific language or framework features should only be required when the target repository explicitly supports them.

## Important limitation

Copilot Code Review is non-deterministic. Custom instructions and skills improve review quality but do not guarantee that every rule will run or that every finding will be correct. AI review should complement analyzers, tests, security tooling, benchmarks, and human review—not replace them.
