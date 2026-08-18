---
applyTo: "**/*.csproj,**/*.props,**/*.targets,**/Directory.Build.*,**/Directory.Packages.props,**/global.json,**/*.sln,**/*.slnx"
---

# .NET Project System Review Guidelines

Apply these rules to MSBuild, SDK, NuGet, solution, and repository-wide .NET configuration changes.

## Compatibility

- Verify `TargetFramework`, `TargetFrameworks`, `LangVersion`, SDK selection, runtime identifiers, and package versions remain compatible with the repository's supported build/deployment matrix.
- Treat target-framework or language-version increases as compatibility changes, not formatting changes.
- Check multi-targeted projects for conditions that accidentally apply only to one TFM or to all TFMs.

## Package references

- Check accidental major-version upgrades, prerelease dependencies, duplicated/conflicting versions, and packages moved between direct/transitive ownership.
- Respect Central Package Management when `Directory.Packages.props` is present.
- Review `PrivateAssets`, `IncludeAssets`, and `ExcludeAssets` when package flow to consumers matters.
- For libraries, check whether new dependencies unintentionally become part of the public/transitive dependency surface.

## MSBuild properties and conditions

- Check conditions for configuration, platform, target framework, operating system, and property evaluation mistakes.
- Avoid duplicating properties already centrally defined unless the project intentionally overrides them.
- Check item globs/removals for unexpectedly including generated files, secrets, artifacts, or large directory trees.
- Review custom targets for ordering (`BeforeTargets`/`AfterTargets`/dependencies), incremental-build inputs/outputs, and cross-platform shell assumptions.

## Build and publish behavior

- Check changes to trimming, AOT, single-file publishing, self-contained deployment, ready-to-run, container publishing, analyzers, nullable, warnings-as-errors, and deterministic builds for unintended production impact.
- Check content/copy settings (`CopyToOutputDirectory`, `CopyToPublishDirectory`) for missing or unintentionally published files.
- Do not require a specific publish model unless the repository documents it.

## NuGet packaging

For packable libraries, review:

- package ID/version metadata changes;
- target frameworks and dependency groups;
- accidentally packaged internal files;
- symbols/source metadata when already part of the project's packaging standard;
- public API compatibility when packaging changes accompany code changes.

## Do not report

- XML ordering or formatting preferences without an established repository convention.
- SDK-style simplifications that do not improve correctness or maintainability.
- central package management, solution filters, `.slnx`, or other project-system features merely because they are newer.
