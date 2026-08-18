---
applyTo: "**/*.cs,**/*.sql"
---

# Persistence Review Guidelines

Apply these checks when changed code accesses a database through EF Core, Dapper, ADO.NET, raw SQL, or another persistence abstraction.

## Correctness and safety

- Parameterize user-controlled values. Never build executable SQL by concatenating or interpolating untrusted values.
- Review transaction boundaries for partial writes, duplicate effects, inconsistent reads, and incorrect connection/transaction ownership.
- Check command and query cancellation when database work participates in a cancellable request or background operation.
- Verify connection, command, reader, and transaction lifetimes are disposed correctly when owned by the changed code.
- Check result cardinality assumptions: zero, one, or many rows must match the API and domain contract.

## Query behavior

- Look for N+1 access, unbounded reads, accidental full-table scans, unnecessary columns, client-side filtering, repeated queries, and query-per-item loops.
- Treat index recommendations as hypotheses unless the query shape and workload provide enough evidence. Do not claim an index is missing without schema or execution-plan context.
- Check pagination for deterministic ordering and realistic large-data behavior.
- Check large `IN` lists, bulk writes, batching, and parameter limits when input cardinality can grow.

## Data contracts

- Review nullability, precision/scale, date/time semantics, enum/value conversions, identifiers, and concurrency tokens when mappings change.
- Check schema or migration changes for backward/forward compatibility with rolling deployments when relevant.
- Flag destructive migration behavior or irreversible data transformations when there is a plausible production data-loss risk.

## Performance discipline

- Distinguish correctness defects from tuning opportunities.
- Do not recommend caching, compiled queries, raw SQL, batching, or micro-optimizations without a concrete repeated-cost or hot-path reason.
- Prefer fewer round trips and bounded data transfer when doing so preserves correctness and clarity.

## Specialist guidance

When relevant, load the EF Core or Dapper reference from `.github/skills/code-review/references/` for deeper checks.
