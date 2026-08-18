<p align="center">
  <strong>English</strong> · <a href="./copilot-instructions.pt-BR.md">Português (Brasil)</a>
</p>

# Copilot Code Review Instructions

> Documentation for this language: [`README.md`](../README.md)

When performing a code review in a .NET repository, use the review workflow in `.github/skills/code-review/SKILL.md` when it is relevant.

## Review contract

- Review the pull request diff first. Read surrounding code only to understand behavior, contracts, invariants, and repository conventions.
- Report only high-confidence findings with a concrete failure mode, security risk, compatibility risk, operational risk, performance cost, or maintainability cost.
- Focus on problems introduced by the pull request. Do not report unrelated legacy problems unless the change exposes, relies on, or materially worsens them.
- Prefer a small number of actionable findings over speculative or stylistic commentary.
- Do not duplicate findings that share the same root cause.
- Do not use code review as a formatting or naming linter when `.editorconfig`, analyzers, formatters, or established repository conventions already govern the rule.
- Do not require a design pattern merely because it is considered a best practice. Recommend an abstraction or pattern only when it solves a concrete problem demonstrated by the changed code.
- Respect the target framework and language version declared by the repository. Do not require newer .NET or C# features unless the project supports them.
- Treat generated code, migrations, snapshots, lock files, vendored code, and generated clients as special cases. Review them only for issues appropriate to their role.

## Priorities

Review in this order:

1. Correctness and data integrity.
2. Security and authorization.
3. Breaking changes and contract compatibility.
4. Concurrency, async behavior, cancellation, and resource lifetime.
5. Persistence and transaction correctness.
6. Production performance and scalability risks.
7. Test coverage of changed behavior and regression risk.
8. Architecture and maintainability when there is concrete impact.
9. Readability only when it materially increases defect risk.

## Severity

Use the following severity labels:

- **BLOCKER** — merging would create an immediate and serious risk such as exploitable security exposure, data loss/corruption, deadlock, severe correctness failure, or an incompatible contract break.
- **HIGH** — likely production defect, authorization failure, serious resource/concurrency issue, or major regression.
- **MEDIUM** — concrete maintainability, performance, testability, or architectural problem that should be addressed but does not necessarily make the change unsafe to merge.
- **LOW** — evidence-backed improvement with meaningful value. Never use LOW for personal style preference.

## Finding quality

Every finding should identify:

- the affected file and changed line when possible;
- the severity and category;
- the concrete problem;
- why it matters in this code path;
- evidence from the diff or surrounding implementation;
- the smallest practical fix.

Do not claim runtime behavior, database behavior, framework guarantees, thread safety, allocation cost, or security impact unless the claim is supported by the code or established platform behavior.

## Positive results

Do not manufacture comments simply to produce output. If the pull request has no material issues, returning no findings is preferable to low-value suggestions.
