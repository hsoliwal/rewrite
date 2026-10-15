# Core and support: package responsibilities

[Architecture index](README.md) · [Module guide](modules.md) · [Coverage ledger](coverage.md)

Source baseline: `0c82f71bc8bb215a8194db59a3784551d6a60e99`. Entries describe source-directory groups and observed declarations. Java/Kotlin/Scala namespaces can span languages/artifacts; Go uses import paths, C# namespaces, Python import packages and TypeScript file modules. One representative declaration was read per group, so these are **partial responsibility summaries**, not an exhaustive namespace/member census. Listed surface names are examples present in the tree, not a claim that every type is public. Existing API documentation remains authoritative except where a specific drift is recorded.

Every entry retains its exact source directory and representative evidence in [coverage](coverage.md). The linked package-info files often contain only nullness declarations. All tests are source references, **not executed receipts**. Public-member parameters/returns/errors, side effects, lifecycle, dependency edges and individual Python/TypeScript file-module semantics still need full reconciliation; this common gap applies to every entry below.

## .m3/openrewrite-recipes

### com.synexia.m3.bootstrap

Java recipe-first bootstrap sidecar: one recipe emits structural atom rows; the task recipe validates a bounded regular file's exact SHA-256 before returning that inventory recipe.

tooling; 2 source files. Surface: `M3BootstrapInventoryRecipe`, `M3BootstrapTaskRecipe`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/.m3/openrewrite-recipes/src/main/java/com/synexia/m3/bootstrap/M3BootstrapTaskRecipe.java); [test-source reference](../../.m3/openrewrite-recipes/src/test/java/com/synexia/m3/bootstrap/M3BootstrapInventoryRecipeTest.java).

## buildSrc

### org.openrewrite.codegen.dsl

Gradle task implementation that inspects visitor bytecode through ASM and writes generated language-scope Kotlin files; it cleans stale generated Scope.kt files in its output directory.

tooling; 1 source files. Surface: `GenerateLanguageScopesTask`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/buildSrc/src/main/kotlin/org/openrewrite/codegen/dsl/GenerateLanguageScopesTask.kt); test relationship unresolved.

## rewrite-benchmarks

### org.openrewrite.benchmarks.core

JMH experiments for core operations. Benchmark source defines workloads/settings; no speed, allocation or scalability result was measured in this review.

benchmark; 5 source files. Surface: `AdaptiveRadixTreeBenchmark`, `GitProvenanceBenchmark`, `SemverVersionSelectorBenchmark`, `TreeRandomIdBenchmark`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-benchmarks/src/jmh/java/org/openrewrite/benchmarks/core/TreeRandomIdBenchmark.java); [package annotations](../../rewrite-benchmarks/src/jmh/java/org/openrewrite/benchmarks/core/package-info.java); test relationship unresolved.

### org.openrewrite.benchmarks.java

JMH experiments for java operations. Benchmark source defines workloads/settings; no speed, allocation or scalability result was measured in this review.

benchmark; 20 source files. Surface: `ChangeMethodTargetToStaticBenchmark`, `ChangeTypeBenchmark`, `FindTypesBenchmark`, `HasMethodUseBenchmark`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-benchmarks/src/jmh/java/org/openrewrite/benchmarks/java/JavaSourceSetBenchmark.java); [package annotations](../../rewrite-benchmarks/src/jmh/java/org/openrewrite/benchmarks/java/package-info.java); test relationship unresolved.

### org.openrewrite.benchmarks.maven

JMH experiments for maven operations. Benchmark source defines workloads/settings; no speed, allocation or scalability result was measured in this review.

benchmark; 2 source files. Surface: `FindDependencyBenchmark`, `MavenParserBenchmark`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-benchmarks/src/jmh/java/org/openrewrite/benchmarks/maven/MavenParserBenchmark.java); test relationship unresolved.

### org.openrewrite.benchmarks.recipe

JMH experiments for recipe operations. Benchmark source defines workloads/settings; no speed, allocation or scalability result was measured in this review.

benchmark; 1 source files. Surface: `RecipeLoadingBenchmark`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-benchmarks/src/jmh/java/org/openrewrite/benchmarks/recipe/RecipeLoadingBenchmark.java); test relationship unresolved.

## rewrite-core

### org.openrewrite

Core contracts and cross-cutting recipe/execution helpers; this namespace is split across core, test and Kotlin DSL artifacts, so resolve the owning module before choosing an API.

production; 71 source files. Surface: `AbstractRecipe`, `AddToGitignore`, `Changeset`, `Checksum`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/Contributor.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/package-info.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/package-info.java).

### org.openrewrite.binary

Opaque byte-preserving SourceFile support; BinaryParser wraps accepted input rather than interpreting a language grammar.

production; 4 source files. Surface: `Binary`, `BinaryParser`, `BinaryVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/binary/BinaryParser.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/binary/package-info.java); test relationship unresolved.

### org.openrewrite.config

Recipe/style discovery, declarative configuration and descriptors; reflection/loading failures are surfaced through recipe introspection exceptions.

production; 17 source files. Surface: `CategoryDescriptor`, `CategoryTree`, `ClasspathScanningLoader`, `ColumnDescriptor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/config/RecipeIntrospectionException.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/config/package-info.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/config/AbstractRecipeFixtures.java).

### org.openrewrite.format

Shared line-break utilities used below language-specific formatting policies.

production; 1 source files. Surface: `LineBreaks`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/format/LineBreaks.java); test relationship unresolved.

### org.openrewrite.internal

Implementation support and services used by parsers/recipes; internal names are not a blanket stable extension contract.

production; 31 source files. Surface: `AdaptiveRadixTree`, `BackwardCompatibleObjectIdModule`, `CommentService`, `EncodingDetectingInputStream`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/internal/NamingService.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/internal/package-info.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/internal/ReflectionUtilsTest.java).

### org.openrewrite.internal.lang

Legacy nullness annotations and helpers; the declarations reference JSpecify and carry migration/deprecation guidance.

production; 6 source files. Surface: `NonNull`, `NonNullApi`, `NonNullFields`, `NullFields`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/internal/lang/NonNullFields.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/internal/lang/NullUtilsTest.java).

### org.openrewrite.ipc.http

HTTP request/response transport abstractions and status classification; implementations perform network I/O.

production; 4 source files. Surface: `HttpSender`, `HttpStatusClass`, `HttpUrlConnectionSender`, `ProxyStatus`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/ipc/http/HttpStatusClass.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/ipc/http/HttpUrlConnectionSenderTest.java).

### org.openrewrite.marker

Metadata attached to source trees, including provenance and source-set identity; metadata is distinct from child syntax nodes.

production; 23 source files. Surface: `AlreadyReplaced`, `BuildMetadata`, `BuildTool`, `BuildToolFailure`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/marker/SourceSet.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/marker/package-info.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/marker/GitTreeEntrySerializationTest.java).

### org.openrewrite.marker.ci

CI/build-environment provenance and configuration handling; incomplete Git configuration has its own exception type.

production; 11 source files. Surface: `BitbucketBuildEnvironment`, `BuildEnvironment`, `CircleCiBuildEnvironment`, `CustomBuildEnvironment`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/marker/ci/IncompleteGitConfigException.java); test relationship unresolved.

### org.openrewrite.marketplace

Recipe bundle discovery/loading, listing and resolution; RecipeBundleResolver is AutoCloseable, so resolver lifecycle belongs to its caller.

production; 16 source files. Surface: `RecipeBundle`, `RecipeBundleReader`, `RecipeBundleResolver`, `RecipeClassLoader`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/marketplace/RecipeBundleResolver.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/marketplace/package-info.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/marketplace/package-info.java).

### org.openrewrite.quark

Minimal source-file placeholders for files not modeled by a richer parser; QuarkParser retains file identity without claiming syntax semantics.

production; 5 source files. Surface: `Quark`, `QuarkParser`, `QuarkPrinter`, `QuarkVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/quark/QuarkParser.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/quark/package-info.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/quark/package-info.java).

### org.openrewrite.remote

Remote source/artifact representations and cache/visitor support; retrieval and cache effects depend on the concrete Remote implementation.

production; 9 source files. Surface: `LocalRemoteArtifactCache`, `Remote`, `RemoteArchive`, `RemoteArtifactCache`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/remote/RemoteVisitor.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/remote/package-info.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/remote/BlackHoleOutputStream.java).

### org.openrewrite.rpc

Cross-process recipe/tree protocol and lifecycle support; process managers retain one RPC instance per thread until shutdown.

production; 15 source files. Surface: `DynamicDispatchRpcCodec`, `Reference`, `RewriteRpc`, `RewriteRpcExecutionContextView`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/rpc/RpcMarker.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/rpc/package-info.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/rpc/package-info.java).

### org.openrewrite.rpc.internal

RPC implementation helpers, including tree-type preconditions that avoid sending irrelevant source files to a peer.

production; 3 source files. Surface: `FindTreesOfType`, `PreparedRecipeCache`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/rpc/internal/FindTreesOfType.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/rpc/internal/package-info.java); test relationship unresolved.

### org.openrewrite.rpc.request

Request and response objects for parsing, visits, generation, object transfer and cache management; wire compatibility matters across peer languages.

production; 19 source files. Surface: `BatchVisit`, `BatchVisitResponse`, `Evict`, `Generate`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/rpc/request/RpcRequest.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/rpc/request/package-info.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/rpc/request/SetDataTableStoreTest.java).

### org.openrewrite.scheduling

Recipe-cycle/stack scheduling state and execution-context wrappers; message changes participate in recipe-run behavior.

production; 6 source files. Surface: `RecipePath`, `RecipeRunCycle`, `RecipeStack`, `WatchableExecutionContext`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/scheduling/WatchableExecutionContext.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/scheduling/package-info.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/scheduling/RecipePathTest.java).

### org.openrewrite.search

Cross-language source/provenance searches that report findings through recipe results and markers.

production; 5 source files. Surface: `FindBuildMetadata`, `FindCommitters`, `FindParseToPrintInequality`, `RepositoryContainsFile`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/search/FindBuildMetadata.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/search/package-info.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/search/FindBuildMetadataTest.java).

### org.openrewrite.semver

Version selectors, comparators and validation with bounded memoization for repeated selector/version work.

production; 19 source files. Surface: `CaretRange`, `DependencyMatcher`, `ExactVersion`, `ExactVersionWithPattern`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/semver/LruCache.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/semver/package-info.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/semver/LatestMinorTest.java).

### org.openrewrite.shell.exec

Shell-execution abstraction; implementations introduce process/environment effects and are not pure tree visitors.

production; 1 source files. Surface: `ShellExecutor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/shell/exec/ShellExecutor.java); test relationship unresolved.

### org.openrewrite.style

Named formatting/style metadata and reusable policy values; concrete language styles extend this shared layer.

production; 6 source files. Surface: `GeneralFormatStyle`, `LineWrapSetting`, `NamedStyles`, `Style`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/style/LineWrapSetting.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/style/package-info.java); test relationship unresolved.

### org.openrewrite.table

Framework data-table schemas for findings, parse failures, provenance and run statistics; the configured DataTableStore owns storage.

production; 18 source files. Surface: `ClasspathReport`, `CollidingSourceFiles`, `CommitsByDay`, `DeserializationErrorTable`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/table/TextMatches.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/table/package-info.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/table/package-info.java).

### org.openrewrite.template

Source-template and coordinate interfaces linking replacement text to insertion/replacement positions in a tree.

production; 2 source files. Surface: `Coordinates`, `SourceTemplate`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/template/SourceTemplate.java); test relationship unresolved.

### org.openrewrite.text

Plain-text parser/tree and text-file recipes; downcasting a richer SourceFile produces text and drops its language-specific tree model.

production; 15 source files. Surface: `AppendToTextFile`, `ChangeText`, `CreateTextFile`, `EndOfLineAtEndOfFile`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/text/PlainTextParser.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/text/package-info.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/text/RemoveByteOrderMarkTest.java).

### org.openrewrite.text.trait

References in service-provider text files, allowing reference-aware changes without a Java compilation-unit tree.

production; 1 source files. Surface: `ServiceProviderReference`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/text/trait/ServiceProviderReference.java); test relationship unresolved.

### org.openrewrite.trait

Cursor-backed semantic views and matchers over syntax; visitor functions compose a trait match with recipe logic.

production; 8 source files. Surface: `Comments`, `Reference`, `SimpleTraitMatcher`, `Trait`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/trait/VisitFunction.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/trait/package-info.java); [test-source reference](../../rewrite-core/src/test/java/org/openrewrite/trait/CommentsTest.java).

### org.openrewrite.tree

Parse-error representation and parsing callbacks/context; a ParseError is a returned source-tree outcome, not proof of successful parsing.

production; 4 source files. Surface: `ParseError`, `ParsingEventListener`, `ParsingExecutionContextView`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/tree/ParsingEventListener.java); [package annotations](../../rewrite-core/src/main/java/org/openrewrite/tree/package-info.java); test relationship unresolved.

## rewrite-test

### org.openrewrite

Test annotations and framework integration helpers, including the deprecated Example annotation; consumers should follow the replacement named in its Javadoc.

production; 4 source files. Surface: `DebugOnly`, `Example`, `Issue`, `MetricsExtension`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-test/src/main/java/org/openrewrite/Example.java); [test-source reference](../../rewrite-test/src/test/java/org/openrewrite/RenameFileTest.java).

### org.openrewrite.test

Exports RewriteTest, RecipeSpec and SourceSpec support for parsing before-text, applying recipes and comparing expected after-text. It is a library consumed by other test suites, not an application. Its JUnit and AssertJ dependencies belong to the test-authoring surface. Runner overrides can select a different execution backend; a test filename or a configured runner is not a receipt that the backend ran.

production; 16 source files. Surface: `AdHocRecipe`, `AdHocScanningRecipe`, `Dir`, `LargeSourceSetCheckingExpectedCycles`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-test/src/main/java/org/openrewrite/test/UncheckedSupplier.java); [package annotations](../../rewrite-test/src/main/java/org/openrewrite/test/package-info.java); [test-source reference](../../rewrite-test/src/test/java/org/openrewrite/test/TypeValidationTest.java).

### org.openrewrite.test.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 1 source files. Surface: `ThrowingUnaryOperator`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-test/src/main/java/org/openrewrite/test/internal/ThrowingUnaryOperator.java); [test-source reference](../../rewrite-test/src/test/java/org/openrewrite/test/internal/RewriteTestTest.java).

## rewrite-bom

This platform project has no source namespace. Its [dependency-constraint contract](modules.md#rewrite-bom) is the relevant surface.
