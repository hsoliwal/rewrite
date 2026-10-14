# Architecture and documentation coverage

This guide describes the public [hsoliwal/rewrite](https://github.com/hsoliwal/rewrite) fork of [openrewrite/rewrite](https://github.com/openrewrite/rewrite). Source observations are pinned to **0c82f71bc8bb215a8194db59a3784551d6a60e99**, the existing documentation PR head inspected on 14 October 2026. Its Git tree is **066b2180752282207da5e2a9a98d192e2b7a74df**. Later documentation commits do not change that source baseline.

## Read in this order

1. [Project and module guide](modules.md): every explicitly configured Gradle project, nested language build, build tooling and M3 sidecar
2. [Core API contracts](api-contracts.md): parsing, visitor state, table resources, RPC lifecycle and Kotlin DSL constraints
3. Package guides: [core/support](packages-core.md), [JVM/build systems](packages-jvm.md), [formats](packages-formats.md), [polyglot runtimes](packages-polyglot.md)
4. [Coverage ledger and remaining work](coverage.md): what was inventoried, what was read, and what is still partial

Use the [contributor guide](../../CONTRIBUTING.md) for toolchains and focused checks, and the established [architecture decisions](../adr/README.md) for design history. Existing inline Javadocs, KDocs, module instructions and fixture catalogs remain the detailed API sources; this guide links them instead of copying them into a second API reference.

## Architectural flow

- A concrete Parser accepts inputs and produces SourceFile trees. Tree identity, syntax, formatting, comments and type information are represented by the relevant language model; parse failures can also be represented as source files.
- Recipe supplies transformation visitors or child recipes. TreeVisitor traverses a tree with a cursor and context; language visitors implement syntax-specific dispatch. Scanning recipes additionally accumulate cross-file information before edits/generation.
- Result/RecipeRun and data tables expose changed files and analysis findings. Storage and filesystem/network effects depend on the concrete implementation; the API does not make every recipe pure.
- JVM parsers use compiler or grammar front ends. C#, Go, JavaScript and Python also have native runtimes connected by RPC. Trees, marker codecs, type names and request shapes must remain aligned on both sides.
- rewrite-test and native test harnesses express before/after and parse/print expectations. TCK suites, integration suites, optional corpus tests and benchmarks have different scopes.

These relationships are supported by [Parser](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/Parser.java), [Recipe](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/Recipe.java), [TreeVisitor](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/TreeVisitor.java), [DataTableStore](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/DataTableStore.java) and the module manifests linked below. They are source observations, not runtime verification.

## Build boundaries

[settings.gradle.kts](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/settings.gradle.kts) declares **34 distinct subproject names**, including two colon-qualified tooling-model projects. The parent `rewrite-gradle-tooling-model` is a structural Gradle container rather than another independently declared implementation module. The root build, automatic `buildSrc` build, nested .NET/npm/Python/Go manifests, and two M3 Maven entry points are accounted for separately. Test fixture manifests are retained as fixture evidence rather than silently promoted to shipping products.

IDE.properties is consulted only during IDEA import/sync. Ordinary command-line settings additionally include Java 8/11/17 parsers. Main source placement alone does not determine production status: the Java TCK packages reusable tests under main, and the benchmarks use a jmh source set.

## M3 implementation boundary

The fork README describes an experimental direction. Concrete code observed here includes the small [bootstrap recipe sidecar](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/.m3/openrewrite-recipes) and the Kotlin recipe-authoring DSL. The sidecar's task recipe checks a task-crate file/hash before exposing a read-only inventory recipe. This does not establish a complete semantic-proof engine, universal transformation coverage, runtime qualification, or successful execution of the separate atomize/patternize runner. The checked-in [sidecar](../../.m3/README.md) and [analysis-runner](../../.m3/atomize-patternize/README.md) instructions have their own bounded roles; they were not executed.

## Evidence and licensing

Only public repository sources and existing public documentation are used in these pages. Upstream authorship, source headers, root LICENSE and module licenses are preserved. Android, C#, Go, JavaScript, Python, Ruby and Scala have separate LICENSE.md files; consult those terms rather than applying the root license to every component. [Contributor licensing guidance](../../CONTRIBUTING.md#licenses-and-review-evidence) links the rights boundary.

This is a **partial, source-reviewed documentation draft**. The complete returned tree has 7,144 entries, including 5,916 blobs. Tree accounting is not semantic package coverage. No build, test, benchmark, generator, linter, rendered documentation or runtime check was run for this extension. See the ledger before using a coverage number or a test link as evidence.
