# Important API contracts and review limits

[Architecture index](README.md) · [Modules](modules.md) · [Coverage](coverage.md)

These notes describe selected implementation/declaration bodies at `0c82f71bc8bb215a8194db59a3784551d6a60e99`. They do not replace the inline API reference, and they do not claim a method-by-method review of every exported member. Test links are locations to inspect/run, not passing receipts.

## Parser and input ownership

[org.openrewrite.Parser](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/Parser.java) defines `Stream<SourceFile> parseInputs(Iterable<Input> sources, Path relativeTo, ExecutionContext ctx)`. `relativeTo` is nullable. Its declaration says each input source is closed at the conclusion of parsing; concrete parsers own implementing that contract. Path-based convenience parsing supplies buffered file streams and wraps open failures in UncheckedIOException. Input equality/hashCode use the path, not a content digest.

`accept(Input)` accepts synthetic inputs or delegates to the path predicate. This matters for in-memory snippets: path acceptance alone is not the whole input-filter contract. Charset comes from ParsingExecutionContextView or defaults to UTF-8. `requirePrintEqualsInput` checks the context flag (default true); a mismatch returns a ParseError with a diff and the erroneous tree, rather than a successful parse result. Consumers must inspect returned source-file types and execution-context errors.

Reference: [ParserTest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/test/java/org/openrewrite/ParserTest.java). Per-language acceptance, parse recovery, type attribution and reset behavior remain defined by that parser, not by a blanket promise in this guide.

## Recipe composition and cycles

[org.openrewrite.Recipe](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/Recipe.java) provides a no-op default `getVisitor()`. Its `run(before, ctx)` overload delegates to a maximum of three cycles, and the overload accepting maxCycles defaults minCycles to one before passing to RecipeScheduler. This is an invocation default, not a guarantee that every recipe runs three times or converges in three cycles.

`getRecipeList`/buildRecipeList compose recipes. Default validation inspects required fields via nullness annotations; custom recipes can strengthen validation. Error handling, scan state and external effects depend on the concrete recipe and ExecutionContext. Reference: [RecipeRunTest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/test/java/org/openrewrite/RecipeRunTest.java) and [ExecutionContext](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/ExecutionContext.java).

## Visitor cursor and lifecycle

[TreeVisitor<T,P>](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/TreeVisitor.java) owns a mutable cursor, visit counter and after-visit list. A visitor instance is stateful; the existence of immutable tree values does not make arbitrary concurrent reuse of a visitor safe. `P` is passed through the traversal and may itself be mutable.

`updateCursor(currentValue)` requires the existing cursor value to be a Tree and the replacement to retain the same tree ID. Violations throw IllegalArgumentException; a null replacement is rejected. `doAfterVisit` queues a visitor for the whole source-file traversal's completion, for operations such as import cleanup or formatting. It does not create a new persistent child recipe. `visitNonNull` asserts that the caller already knows the visit cannot return null; it is not a conversion from a nullable transformation to a guaranteed non-null result.

Follow the language-specific visitor and existing before/after tests when replacing or removing nodes. Cursor parenthood, padding, formatting and type attribution are observable parts of a transformation even when its Java method signature is unchanged.

## Data-table storage and stream resources

[DataTableStore](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/DataTableStore.java) separates row storage from a DataTable definition. A bucket is keyed by the table's class name plus group/instance identity according to the table type. In-memory and CSV stores have different persistence/resource effects; `noop()` intentionally drops inserts and returns empty results.

All `getRows` overloads carry MustBeClosed. Close returned streams (for example with try-with-resources), including typed overloads. The old string-name overload is deprecated in favor of class-based deserialization. Do not read an empty result as proof that a recipe did not find anything when a no-op store was configured. Reference: [DataTableStoreTest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/test/java/org/openrewrite/DataTableStoreTest.java) and [RPC table-store ADR](../adr/0008-rpc-data-table-stores.md).

## RPC process ownership

[RewriteRpcProcessManager<R>](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/rpc/RewriteRpcProcessManager.java) stores both the RPC instance and factory in ThreadLocal values. `get()` may return null. `getOrStart()` invokes the current thread's factory only if that thread has no instance. `setFactory()` changes the current thread's factory; it does not replace an already started instance. `reset()` invokes the current instance's reset without removing it, while `shutdown()` shuts it down and removes the thread-local instance. These operations are not a global shutdown/reset across all threads.

The native process and peer codecs are additional failure boundaries. Refer to each module's bounded-timeout test instructions. Sender/receiver alignment, request schemas, process launch/version resolution and error recovery require integration tests; they were not validated by this source review.

## GoModParser: current implementation versus stale comment

[GoModParser.parseInputs](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/src/main/java/org/openrewrite/golang/GoModParser.java) calls GoRewriteRpc.getOrStart(), parses with the `GoMod` source-file type, and then enriches the resolution marker with hashes from a readable sibling go.sum. `accept(Path)` selects the filename go.mod. The enrichment leaves other source-file types unchanged, requires a nonempty module path, and returns the original source when no hashes were found. A disk path matters to sibling lookup; in-memory content alone cannot supply a sibling file.

The class-level Javadoc still describes a PlainText document and an includeSumHashes option. That is stale relative to the inspected parseInputs path. PlainText-based parseMarker helpers remain in the file, but they do not establish the current public parse result. This Markdown guide records the distinction; the Java source was not edited. Reference: [GoModParserTest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/src/integTest/java/org/openrewrite/golang/GoModParserTest.java).

## Kotlin recipe DSL: lifecycle and compiler requirement

[recipe / recipes / RecipeBuilder](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/kotlin/org/openrewrite/RecipeDsl.kt) is an existing authoring surface, not a proposed module invented for this documentation. `recipe(displayName, description, tags, estimatedEffortPerOccurrence, block)` runs its construction block once, then captured phase blocks create visitors during lifecycle calls. Duplicate edit/generate declarations and incompatible scan/bare-edit combinations are rejected by require checks.

`recipes(displayName, description, vararg recipes)` creates an ordered composite without an edit phase of its own. The imperative edit/visitor path works as a JVM builder; the declarative `rewrite { } to { }` path requires the K2 plugin, and the untransformed runtime stub raises an error. Generated language scopes require their language artifacts on the consuming recipe's classpath. [The build](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/build.gradle.kts) deliberately leaves Gradle scope generation out to avoid a cycle.

References: [RecipeDslSurfaceTest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/test/kotlin/org/openrewrite/RecipeDslSurfaceTest.kt) and [RecipePluginRewriteTest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/test/kotlin/org/openrewrite/kotlin/recipe/RecipePluginRewriteTest.kt). Neither compilation nor plugin integration was run here.

## Ruby parser serialization boundary

[RubyParser](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-ruby/src/main/java/org/openrewrite/ruby/RubyParser.java) shares one Prism instance and synchronizes the call that parses bytes. It configures fragment-friendly parsing and converts Prism errors to RubySyntaxException; parseInputs reports caught failures to the context and returns ParseError. That synchronized call does not establish thread-safety for every downstream visitor or recipe. The parser's filename acceptance and byte/character mapping are broader than a simple .rb suffix. Read [the existing Ruby mapping guide](../../rewrite-ruby/README.md) and [Prism ADR](../adr/0012-ruby-parsing-via-prism.md) before changing it.

## Inline documentation debt

The inspected main-source package-info.java inventory has 191 files; only two contain package-level Javadoc prose (Android traits and the Gradle tooling model). The others provide declarations/annotations but do not establish semantic package documentation. Four directories contain only package-info.java, including a C# remote declaration whose namespace differs from its path. These are explicitly recorded in [coverage](coverage.md).

This pass leaves inline source/Javadocs unchanged. Full reconciliation of parameters, return/error behavior, ownership, concurrency, serialization and test coverage across all public APIs is still open. The package guides are navigation and responsibility summaries with source evidence, not an assertion that this debt is closed.
