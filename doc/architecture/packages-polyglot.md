# Polyglot runtimes: package responsibilities

[Architecture index](README.md) · [Module guide](modules.md) · [Coverage ledger](coverage.md)

Source baseline: `0c82f71bc8bb215a8194db59a3784551d6a60e99`. Entries describe source-directory groups and observed declarations. Java/Kotlin/Scala namespaces can span languages/artifacts; Go uses import paths, C# namespaces, Python import packages and TypeScript file modules. One representative declaration was read per group, so these are **partial responsibility summaries**, not an exhaustive namespace/member census. Listed surface names are examples present in the tree, not a claim that every type is public. Existing API documentation remains authoritative except where a specific drift is recorded.

Every entry retains its exact source directory and representative evidence in [coverage](coverage.md). The linked package-info files often contain only nullness declarations. All tests are source references, **not executed receipts**. Public-member parameters/returns/errors, side effects, lifecycle, dependency edges and individual Python/TypeScript file-module semantics still need full reconciliation; this common gap applies to every entry below.

## rewrite-csharp

### OpenRewrite.CSharp

Roslyn-backed C# parsing, tree operations, project loading and recipe activation in the native SDK.

production; 35 source files. Surface: `AddUsing`, `AssemblyTypeEnumerator`, `AttributeMapping`, `AttributeMatcher`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite/CSharp/CoreCSharpRecipeActivator.cs); test relationship unresolved.

### OpenRewrite.CSharp.Format

C# auto-formatting and Roslyn/EditorConfig style reconciliation for tree edits.

production; 5 source files. Surface: `AutoFormatVisitor`, `EditorConfigResolver`, `MinimumViableSpacingVisitor`, `RoslynFormatter`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite/CSharp/Format/AutoFormatVisitor.cs); test relationship unresolved.

### OpenRewrite.CSharp.NuGet

NuGet resolution and source-failure classification; restore can use configured package sources and local caches.

production; 2 source files. Surface: `NuGetResolver`, `NuGetSourceFailures`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite/CSharp/NuGet/NuGetSourceFailures.cs); test relationship unresolved.

### OpenRewrite.CSharp.Rpc

Native C# RPC service, serializers and plugin load context; loaded recipes share the SDK contract across assemblies.

production; 7 source files. Surface: `CSharpReceiver`, `CSharpSender`, `CsDocCommentReceiver`, `CsDocCommentSender`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite/CSharp/Rpc/PluginLoadContext.cs); test relationship unresolved.

### OpenRewrite.CSharp.Template

C# template/pattern coordinates, capture constraints and matching; placeholders are internal to the template pipeline.

production; 13 source files. Surface: `CSharpCoordinates`, `CSharpPattern`, `CSharpTemplate`, `Capture`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite/CSharp/Template/Placeholder.cs); test relationship unresolved.

### OpenRewrite.Core

Native C# tree, visitor, recipe, execution and descriptor framework mirroring the cross-language contract.

production; 42 source files. Surface: `CategoryAttribute`, `CategoryDescriptor`, `CategoryDescriptorAttribute`, `Check`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite/Core/CategoryDescriptor.cs); test relationship unresolved.

### OpenRewrite.Core.Rpc

C# RPC queue/codec and object-reference support; message/type registration must agree with peers.

production; 8 source files. Surface: `IRpcCodec`, `Reference`, `RpcJson`, `RpcObjectData`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite/Core/Rpc/IRpcCodec.cs); test relationship unresolved.

### OpenRewrite.Java

Shared J-style tree and type shapes used by C# syntax that has equivalent structure.

production; 4 source files. Surface: `J.Helpers`, `J`, `JavaType`, `JavaVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite/Java/J.Helpers.cs); test relationship unresolved.

### OpenRewrite.Java.Rpc

Serialization/deserialization of shared J shapes between the C# SDK and Java peer.

production; 2 source files. Surface: `JavaReceiver`, `JavaSender`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite/Java/Rpc/JavaSender.cs); test relationship unresolved.

### OpenRewrite.Java.Search

Shared-J method/type/source-file preconditions for restricting native recipe visits.

production; 4 source files. Surface: `IsSourceFile`, `Preconditions`, `UsesMethod`, `UsesType`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite/Java/Search/UsesMethod.cs); test relationship unresolved.

### OpenRewrite.Test

Public C# recipe-test/RPC fixture support shipped with the SDK; the runnable xUnit suite lives in OpenRewrite.Tests.

production; 4 source files. Surface: `RewriteTest`, `RpcFixture`, `RpcRewriteTest`, `Validations`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite/Test/RpcRewriteTest.cs); test relationship unresolved.

### OpenRewrite.Xml

Native XML/C# project parsing and tag visitors used in the C# runtime.

production; 10 source files. Surface: `AddToTagVisitor`, `ChangeTagValueVisitor`, `CsprojParser`, `RemoveContentVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite/Xml/ChangeTagValueVisitor.cs); test relationship unresolved.

### OpenRewrite.Xml.Grammar

ANTLR-generated XML parser interfaces and automata; the owning generation task is in rewrite-xml.

generated; 6 source files. Surface: `XMLLexer`, `XMLParser`, `XMLParserBaseListener`, `XMLParserBaseVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite/Xml/Grammar/XMLParserVisitor.cs); test relationship unresolved.

### OpenRewrite.Xml.Rpc

Native XML tree send/receive support for the cross-language protocol.

production; 2 source files. Surface: `XmlReceiver`, `XmlSender`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite/Xml/Rpc/XmlSender.cs); test relationship unresolved.

### org.openrewrite.csharp

Provides CSharpParser, Cs/Linq trees, Java-side RPC codecs, MSBuild path recipes, styles and service adapters. CSharpParser delegates solution/project parsing to a .NET process. The nested SDK, tool and xUnit projects implement the native side with Roslyn and StreamJsonRpc. csharpBuild, csharpTest and integTest are distinct stages; check includes both native tests and Java-to-C# integration tests.

production; 8 source files. Surface: `Assertions`, `CSharpIsoVisitor`, `CSharpParser`, `CSharpVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/src/main/java/org/openrewrite/csharp/CSharpParser.java); [package annotations](../../rewrite-csharp/src/main/java/org/openrewrite/csharp/package-info.java); [test-source reference](../../rewrite-csharp/src/integTest/java/org/openrewrite/csharp/CSharpVisitorCompletenessTest.java).

### org.openrewrite.csharp.marker

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 19 source files. Surface: `AnonymousMethod`, `AssemblyReferencesMarker`, `ConditionalBranchMarker`, `DirectiveBoundaryMarker`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/src/main/java/org/openrewrite/csharp/marker/OmitBraces.java); [package annotations](../../rewrite-csharp/src/main/java/org/openrewrite/csharp/marker/package-info.java); [test-source reference](../../rewrite-csharp/src/test/java/org/openrewrite/csharp/marker/MSBuildProjectTest.java).

### org.openrewrite.csharp.marketplace

Language-package recipe bundle discovery/resolution that bridges listings and preparation into the owning language runtime.

production; 2 source files. Surface: `NuGetRecipeBundleReader`, `NuGetRecipeBundleResolver`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/src/main/java/org/openrewrite/csharp/marketplace/NuGetRecipeBundleReader.java); test relationship unresolved.

### org.openrewrite.csharp.msbuild

MSBuild/solution path-case reconciliation using a scanning recipe and path index; path attributes must be distinguished from package/assembly identities.

production; 2 source files. Surface: `AlignPathCasing`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/src/main/java/org/openrewrite/csharp/msbuild/AlignPathCasing.java); [package annotations](../../rewrite-csharp/src/main/java/org/openrewrite/csharp/msbuild/package-info.java); [test-source reference](../../rewrite-csharp/src/test/java/org/openrewrite/csharp/msbuild/AlignPathCasingTest.java).

### org.openrewrite.remote.csharp

Declared by the lone package-info.java under csharp/remote; no implementation type is present in that directory. The declared namespace and directory spelling differ.

stub; 1 source files. Surface: declaration-only. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/src/main/java/org/openrewrite/csharp/remote/package-info.java); [package annotations](../../rewrite-csharp/src/main/java/org/openrewrite/csharp/remote/package-info.java); test relationship unresolved.

### org.openrewrite.csharp.rpc

Language RPC façade, process configuration and request/response values for parse/install/visit operations; not every message type is public.

production; 13 source files. Surface: `CSharpReceiver`, `CSharpRewriteRpc`, `CSharpRpcCodec`, `CSharpSender`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/src/main/java/org/openrewrite/csharp/rpc/InstallRecipesByFile.java); [package annotations](../../rewrite-csharp/src/main/java/org/openrewrite/csharp/rpc/package-info.java); [test-source reference](../../rewrite-csharp/src/integTest/java/org/openrewrite/csharp/rpc/EnvServerEntryIntegTest.java).

### org.openrewrite.csharp.service

Language-specific service adapters for comments, imports, naming, formatting or validation; inspect the concrete service's effects before calling it.

production; 4 source files. Surface: `CSharpAutoFormatService`, `CSharpNamingService`, `CSharpWhitespaceValidationService`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/src/main/java/org/openrewrite/csharp/service/CSharpWhitespaceValidationService.java); [package annotations](../../rewrite-csharp/src/main/java/org/openrewrite/csharp/service/package-info.java); test relationship unresolved.

### org.openrewrite.csharp.style

Language style definitions and/or autodetection used by formatting; style values are separate from the visitor that applies them.

production; 1 source files. Surface: `CSharpFormatStyle`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/src/main/java/org/openrewrite/csharp/style/CSharpFormatStyle.java); test relationship unresolved.

### org.openrewrite.csharp.table

Data-table schemas emitted by the module's recipes and analysis; actual row retention/output depends on the configured DataTableStore.

production; 2 source files. Surface: `PathCasingMismatches`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/src/main/java/org/openrewrite/csharp/table/PathCasingMismatches.java); [package annotations](../../rewrite-csharp/src/main/java/org/openrewrite/csharp/table/package-info.java); test relationship unresolved.

### org.openrewrite.csharp.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 9 source files. Surface: `Cs`, `CsContainer`, `CsDocComment`, `CsLeftPadded`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/src/main/java/org/openrewrite/csharp/tree/Implements.java); [package annotations](../../rewrite-csharp/src/main/java/org/openrewrite/csharp/tree/package-info.java); test relationship unresolved.

## rewrite-go

### main

Exploration command for native parsing/tree inspection; it is a developer utility rather than the RPC server.

production; 1 source files. Surface: `main`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/cmd/explore/main.go); [test-source reference](../../rewrite-go/test/testdata/printer-corpus/generics/03_multi_param.go).

### main

Command that generates embedded Go export-data artifacts for type attribution; filesystem output is part of its purpose.

production; 2 source files. Surface: `main`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/cmd/goexportdata/main.go); [test-source reference](../../rewrite-go/cmd/goexportdata/main_test.go).

### main

Executable JSON-RPC server for parse, visit and object exchange; stdin/stdout must remain protocol-safe.

production; 17 source files. Surface: `main`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/cmd/rpc/main.go); [test-source reference](../../rewrite-go/cmd/rpc/after_visits_test.go).

### diff

Unified source-diff rendering with configurable output limits; used for reporting transformation differences.

production; 2 source files. Surface: `unified`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/diff/unified.go); [test-source reference](../../rewrite-go/pkg/diff/unified_test.go).

### exportdata

Maps import paths to embedded export-data blobs and supplies a Go types importer over filesystem sets.

production; 2 source files. Surface: `exportdata`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/exportdata/exportdata.go); [test-source reference](../../rewrite-go/pkg/exportdata/exportdata_test.go).

### format

Go whitespace, spacing, indentation and comment formatting visitors; transformations return updated trees.

production; 23 source files. Surface: `auto_format`, `binary_spacing`, `blank_lines`, `doc_comments`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/format/trailing_whitespace.go); [test-source reference](../../rewrite-go/pkg/format/binary_spacing_test.go).

### matcher

Go method/type matching over attributed shared tree types; a syntactic name alone is not full type evidence.

production; 4 source files. Surface: `method_matcher`, `type_utils`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/matcher/method_matcher.go); [test-source reference](../../rewrite-go/pkg/matcher/assignability_test.go).

### parenthesize

Adds parentheses when the expression's cursor position requires them; distinguishes expression and type wrapping.

production; 3 source files. Surface: `parenthesize`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/parenthesize/parenthesize.go); [test-source reference](../../rewrite-go/pkg/parenthesize/golang_cases_test.go).

### parser

Go/go.mod/go.sum parsing and compiler/type-attribution mapping into native lossless tree shapes.

production; 29 source files. Surface: `build_tags`, `exported_types`, `go_parser`, `gomod_lst`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/parser/gosum_lst.go); [test-source reference](../../rewrite-go/pkg/parser/attachment_audit_test.go).

### preconditions

Source-file/type/method search visitors used to filter recipe execution before transformation.

production; 3 source files. Surface: `preconditions`, `search`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/preconditions/search.go); [test-source reference](../../rewrite-go/pkg/preconditions/preconditions_test.go).

### printer

Go and module/checksum-file printers, output capture and marker-printing policy.

production; 6 source files. Surface: `go_printer`, `gomod_printer`, `gosum_printer`, `marker_printer`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/printer/gosum_printer.go); [test-source reference](../../rewrite-go/pkg/printer/parity_test.go).

### recipe

Native recipe registry, execution context, tables and scan/edit lifecycle; ExecutionContext holds mutable messages.

production; 7 source files. Surface: `data_table`, `execution_context`, `recipe`, `registry`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/recipe/execution_context.go); [test-source reference](../../rewrite-go/pkg/recipe/registry_test.go).

### golang

Built-in Go recipes, import operations and language services registered by Activate.

production; 18 source files. Surface: `activate`, `add_import`, `annotation_service`, `auto_format_service`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/recipe/golang/activate.go); test relationship unresolved.

### internal

Import-path/name/group utilities shared by native Go recipe implementations.

production; 2 source files. Surface: `imports`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/recipe/golang/internal/imports.go); [test-source reference](../../rewrite-go/pkg/recipe/golang/internal/imports_test.go).

### installer

Recipe-module installation/workspace support; installation introduces filesystem/process/dependency effects.

production; 2 source files. Surface: `installer`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/recipe/installer/installer.go); [test-source reference](../../rewrite-go/pkg/recipe/installer/installer_test.go).

### rpc

Native queues, object transfer, type/node codecs and protocol helpers; wire shape must match the Java codecs.

production; 48 source files. Surface: `go_receiver`, `go_resolution_result_codec`, `go_sender`, `gomod_codec`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/rpc/node_helpers.go); [test-source reference](../../rewrite-go/pkg/rpc/annotation_rpc_test.go).

### template

Go template parsing, captures, comparisons and type import support; importer reuse is an internal cache.

production; 37 source files. Surface: `capture`, `comparator`, `explain`, `fast_path`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/template/importer_cache.go); [test-source reference](../../rewrite-go/pkg/template/capture_type_test.go).

### test

Native recipe/parse test specifications, attributed-node collection and Java RPC test support.

production; 8 source files. Surface: `collect`, `expect_type`, `export_data`, `java_rpc`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/test/collect.go); [test-source reference](../../rewrite-go/pkg/test/expect_type_test.go).

### golang

Go-specific compilation-unit, go.mod/go.sum and marker structures layered over shared tree interfaces.

production; 6 source files. Surface: `go`, `go_methods`, `go_resolution_result`, `gomod`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/tree/golang/gosum.go); test relationship unresolved.

### java

Native shared J-style tree/type/space/marker and padding structures used by Go syntax.

production; 10 source files. Surface: `equality`, `j`, `j_methods`, `java_type`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/tree/java/padding.go); [test-source reference](../../rewrite-go/pkg/tree/java/space_comment_test.go).

### visitor

Cursor-aware traversal and visitor dispatch; parent relationships distinguish function bodies and initializer wrappers.

production; 9 source files. Surface: `cursor`, `cursor_helpers`, `drain`, `go_visitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg/visitor/cursor_helpers.go); [test-source reference](../../rewrite-go/pkg/visitor/cursor_helpers_test.go).

### org.openrewrite.golang

Java GolangParser/GoModParser/GoSumParser and Go/J tree/codecs bridge to the Go module in this directory. cmd/rpc is the native RPC entry point; pkg/parser, tree, visitor, printer, recipe, template and rpc implement its language runtime. The Gradle build stages Go source and vendored dependencies as resources, builds an RPC executable, runs goTest and defines a separate integTest suite. GoModParser currently returns GoMod through RPC despite its older PlainText class comment.

production; 9 source files. Surface: `Assertions`, `GoFileChecker`, `GoModParser`, `GoModVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/src/main/java/org/openrewrite/golang/GoModParser.java); [test-source reference](../../rewrite-go/src/integTest/java/org/openrewrite/golang/GoSumParserTest.java).

### org.openrewrite.golang.format

Formatting recipes and visitors that adjust spacing, indentation or line breaks in the language model.

production; 5 source files. Surface: `AutoFormatVisitor`, `BlankLinesVisitor`, `NormalizeLineBreaksVisitor`, `RemoveTrailingWhitespaceVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/src/main/java/org/openrewrite/golang/format/NormalizeLineBreaksVisitor.java); [test-source reference](../../rewrite-go/src/integTest/java/org/openrewrite/golang/format/TabsAndIndentsStyleTest.java).

### org.openrewrite.golang.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 3 source files. Surface: `GoExecutor`, `LockFileRegeneration`, `ModuleCache`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/src/main/java/org/openrewrite/golang/internal/GoExecutor.java); [test-source reference](../../rewrite-go/src/test/java/org/openrewrite/golang/internal/ModuleCacheTest.java).

### org.openrewrite.golang.internal.rpc

Tree-specific RPC codecs and send/receive visitors. The inspected codec delegates to matching sender/receiver implementations; their field order/type identities must stay aligned with the native peer.

production; 9 source files. Surface: `GoModRpcCodec`, `GoSumRpcCodec`, `GolangContainerRpcCodec`, `GolangLeftPaddedRpcCodec`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/src/main/java/org/openrewrite/golang/internal/rpc/GolangRpcCodec.java); test relationship unresolved.

### org.openrewrite.golang.marker

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 16 source files. Surface: `Builtin`, `ConstDecl`, `GoProject`, `GoResolutionResult`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/src/main/java/org/openrewrite/golang/marker/ConstDecl.java); test relationship unresolved.

### org.openrewrite.golang.marketplace

Language-package recipe bundle discovery/resolution that bridges listings and preparation into the owning language runtime.

production; 2 source files. Surface: `GolangRecipeBundleReader`, `GolangRecipeBundleResolver`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/src/main/java/org/openrewrite/golang/marketplace/GolangRecipeBundleReader.java); test relationship unresolved.

### org.openrewrite.golang.rpc

Language RPC façade, process configuration and request/response values for parse/install/visit operations; not every message type is public.

production; 9 source files. Surface: `Dependency`, `GoParseRequest`, `GoRewriteRpc`, `GoRpcSourceExtractor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/src/main/java/org/openrewrite/golang/rpc/InstallRecipesByFile.java); [test-source reference](../../rewrite-go/src/integTest/java/org/openrewrite/golang/rpc/PartialTypeAttributionIntegTest.java).

### org.openrewrite.golang.search

Search/precondition recipes that select syntax or dependency findings; result markers/tables are distinct from rewriting matched source.

production; 1 source files. Surface: `DependencyInsight`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/src/main/java/org/openrewrite/golang/search/DependencyInsight.java); [test-source reference](../../rewrite-go/src/integTest/java/org/openrewrite/golang/search/DependencyInsightTest.java).

### org.openrewrite.golang.service

Language-specific service adapters for comments, imports, naming, formatting or validation; inspect the concrete service's effects before calling it.

production; 3 source files. Surface: `GolangAddImport`, `GolangAutoFormatService`, `GolangImportService`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/src/main/java/org/openrewrite/golang/service/GolangAutoFormatService.java); [test-source reference](../../rewrite-go/src/integTest/java/org/openrewrite/golang/service/GolangAddImportTest.java).

### org.openrewrite.golang.table

Data-table schemas emitted by the module's recipes and analysis; actual row retention/output depends on the configured DataTableStore.

production; 1 source files. Surface: `GoDependenciesInUse`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/src/main/java/org/openrewrite/golang/table/GoDependenciesInUse.java); test relationship unresolved.

### org.openrewrite.golang.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 6 source files. Surface: `ChanDirMarker`, `Go`, `GoMod`, `GoModTree`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/src/main/java/org/openrewrite/golang/tree/ChanDirMarker.java); test relationship unresolved.

## rewrite-javascript

### npm area /

Root npm framework exports: recipe, execution, data tables, markers, parsing and tree visiting.

production; 19 source files. Surface: `data-table`, `execution`, `index`, `markers`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/index.ts); test relationship unresolved.

### npm area /java

Shared J tree/type/visitor and protocol exports used by TypeScript language nodes.

production; 9 source files. Surface: `formatting-utils`, `index`, `markers`, `print`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/java/index.ts); test relationship unresolved.

### npm area /javascript

JavaScript/TypeScript tree, parser, visitor, assertions, imports and binding integration.

production; 29 source files. Surface: `add-import`, `amd`, `assertions`, `autodetect`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/javascript/index.ts); test relationship unresolved.

### npm area /javascript/cleanup

Built-in cleanup recipes such as optional chaining, property shorthand and explicit parseInt radix.

production; 4 source files. Surface: `add-parse-int-radix`, `index`, `prefer-optional-chain`, `use-object-property-shorthand`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/javascript/cleanup/index.ts); test relationship unresolved.

### npm area /javascript/format

Native formatting entry points, including auto-format visitors and optional Prettier integration.

production; 8 source files. Surface: `format`, `index`, `minimum-viable-spacing-visitor`, `normalize-whitespace-visitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/javascript/format/index.ts); test relationship unresolved.

### npm area /javascript/migrate/es6

ES6 migration recipes for literal/escape and object syntax.

production; 4 source files. Surface: `index`, `modernize-octal-escape-sequences`, `modernize-octal-literals`, `remove-duplicate-object-keys`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/javascript/migrate/es6/index.ts); test relationship unresolved.

### npm area /javascript/migrate

Migration-category descriptors and grouping; concrete recipes live in the ES6/TypeScript child modules.

production; 1 source files. Surface: `index`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/javascript/migrate/index.ts); test relationship unresolved.

### npm area /javascript/migrate/typescript

TypeScript migration recipes, including export-assignment conversion.

production; 2 source files. Surface: `export-assignment-to-export-default`, `index`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/javascript/migrate/typescript/index.ts); test relationship unresolved.

### npm area /javascript/recipes

Recipe implementations for imports, dependencies, formatting and call-site changes.

production; 6 source files. Surface: `async-callback-in-sync-array-method`, `auto-format`, `change-import`, `dependencies`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/javascript/recipes/index.ts); test relationship unresolved.

### npm area /javascript/search

Dependency, method and type search/precondition exports.

production; 4 source files. Surface: `find-dependency`, `index`, `uses-method`, `uses-type`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/javascript/search/index.ts); test relationship unresolved.

### npm area /javascript/templating

Template construction and pattern/capture exports; exact matching and replacement contracts live in the source types.

production; 12 source files. Surface: `bindings`, `capture`, `comparator`, `engine`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/javascript/templating/index.ts); test relationship unresolved.

### npm area /json

JSON trees, visitor, parser/assertion and protocol support in the native runtime.

production; 7 source files. Surface: `assertions`, `index`, `parser`, `print`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/json/index.ts); test relationship unresolved.

### npm area /rpc

Native server/client, recipe bridge, queue/reference and tracing support.

production; 10 source files. Surface: `index`, `java-recipe`, `java-rpc-client`, `message-decoder`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/rpc/index.ts); test relationship unresolved.

### npm area /rpc/request

Typed protocol requests/responses; object, parse, generate and dependency operations are distinct messages.

production; 18 source files. Surface: `batch-visit`, `dependency-types`, `generate`, `get-languages`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/rpc/request/index.ts); test relationship unresolved.

### npm area /search

Cross-language source-file precondition utilities.

production; 2 source files. Surface: `index`, `is-source-file`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/search/index.ts); test relationship unresolved.

### npm area /test

RecipeSpec-based asynchronous parse/transform/expected-output test harness.

production; 3 source files. Surface: `index`, `java-rpc`, `rewrite-test`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/test/index.ts); test relationship unresolved.

### npm area /text

Plain-text tree, parser and assertion exports.

production; 7 source files. Surface: `assertions`, `index`, `parser`, `print`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/text/index.ts); test relationship unresolved.

### npm area /yaml

YAML tree/parser/assertion and marker exports for native cross-language recipes.

production; 8 source files. Surface: `assertions`, `index`, `markers`, `parser`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/src/yaml/index.ts); test relationship unresolved.

### org.openrewrite.javascript

The Java library provides JS/J trees, JavaScriptParser, RPC/process management, dependency/lock-file operations and marketplace integration. Nested @openrewrite/rewrite contains TypeScript implementations, the rewrite-rpc executable and exported JavaScript, Java, JSON, YAML, text and test surfaces. RPC type names and sender/receiver ordering must match the Java side. Native build scripts delete generated outputs, chmod the server and run npm link; they are not read-only documentation commands.

production; 17 source files. Surface: `AddDependency`, `Assertions`, `ChangeDependency`, `DependencyWorkspace`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/src/main/java/org/openrewrite/javascript/JavaScriptParser.java); [package annotations](../../rewrite-javascript/src/main/java/org/openrewrite/javascript/package-info.java); [test-source reference](../../rewrite-javascript/src/integTest/java/org/openrewrite/javascript/ChangeMethodNameTest.java).

### org.openrewrite.javascript.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 12 source files. Surface: `BunLockAdapter`, `DependencyPathSegment`, `LockFileParser`, `LockFileRegeneration`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/src/main/java/org/openrewrite/javascript/internal/LockFileParser.java); [test-source reference](../../rewrite-javascript/src/test/java/org/openrewrite/javascript/internal/PackageManagerExecutorTest.java).

### org.openrewrite.javascript.internal.lock

Native lock-file delta planning and format-specific patching; LockPatcher accepts a validated edit set and signals EngineFailure for unrepresentable changes.

production; 28 source files. Surface: `BerryZipChecksum`, `BunJson`, `BunLockDiff`, `BunLockPatcher`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/src/main/java/org/openrewrite/javascript/internal/lock/LockPatcher.java); [package annotations](../../rewrite-javascript/src/main/java/org/openrewrite/javascript/internal/lock/package-info.java); [test-source reference](../../rewrite-javascript/src/test/java/org/openrewrite/javascript/internal/lock/ResolveAndPatchLockRegenTest.java).

### org.openrewrite.javascript.internal.registry

npm registry metadata models and access helpers; abbreviated package metadata chooses versions, while full manifests supply per-version fields.

production; 12 source files. Surface: `AbbreviatedPackument`, `EnvExpansion`, `Environment`, `Netrc`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/src/main/java/org/openrewrite/javascript/internal/registry/AbbreviatedPackument.java); [package annotations](../../rewrite-javascript/src/main/java/org/openrewrite/javascript/internal/registry/package-info.java); [test-source reference](../../rewrite-javascript/src/test/java/org/openrewrite/javascript/internal/registry/UrlsTest.java).

### org.openrewrite.javascript.internal.rpc

Tree-specific RPC codecs and send/receive visitors. The inspected codec delegates to matching sender/receiver implementations; their field order/type identities must stay aligned with the native peer.

production; 9 source files. Surface: `JavaScriptContainerRpcCodec`, `JavaScriptLeftPaddedRpcCodec`, `JavaScriptReceiver`, `JavaScriptRightPaddedRpcCodec`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/src/main/java/org/openrewrite/javascript/internal/rpc/JavaScriptRpcCodec.java); [package annotations](../../rewrite-javascript/src/main/java/org/openrewrite/javascript/internal/rpc/package-info.java); test relationship unresolved.

### org.openrewrite.javascript.marker

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 10 source files. Surface: `Braces`, `Comma`, `DelegatedYield`, `FunctionDeclaration`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/src/main/java/org/openrewrite/javascript/marker/Comma.java); [package annotations](../../rewrite-javascript/src/main/java/org/openrewrite/javascript/marker/package-info.java); [test-source reference](../../rewrite-javascript/src/test/java/org/openrewrite/javascript/marker/NodeResolutionResultSerializationTest.java).

### org.openrewrite.javascript.marketplace

Language-package recipe bundle discovery/resolution that bridges listings and preparation into the owning language runtime.

production; 3 source files. Surface: `NpmRecipeBundleReader`, `NpmRecipeBundleResolver`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/src/main/java/org/openrewrite/javascript/marketplace/NpmRecipeBundleReader.java); [package annotations](../../rewrite-javascript/src/main/java/org/openrewrite/javascript/marketplace/package-info.java); test relationship unresolved.

### org.openrewrite.javascript.rpc

Language RPC façade, process configuration and request/response values for parse/install/visit operations; not every message type is public.

production; 8 source files. Surface: `Dependency`, `InstallRecipesByFile`, `InstallRecipesByPackage`, `InstallRecipesResponse`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/src/main/java/org/openrewrite/javascript/rpc/InstallRecipesResponse.java); [package annotations](../../rewrite-javascript/src/main/java/org/openrewrite/javascript/rpc/package-info.java); [test-source reference](../../rewrite-javascript/src/integTest/java/org/openrewrite/javascript/rpc/package-info.java).

### org.openrewrite.javascript.search

Search/precondition recipes that select syntax or dependency findings; result markers/tables are distinct from rewriting matched source.

production; 2 source files. Surface: `DependencyInsight`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/src/main/java/org/openrewrite/javascript/search/DependencyInsight.java); [package annotations](../../rewrite-javascript/src/main/java/org/openrewrite/javascript/search/package-info.java); [test-source reference](../../rewrite-javascript/src/integTest/java/org/openrewrite/javascript/search/DependencyInsightTest.java).

### org.openrewrite.javascript.service

Language-specific service adapters for comments, imports, naming, formatting or validation; inspect the concrete service's effects before calling it.

production; 1 source files. Surface: `JavaScriptAutoFormatService`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/src/main/java/org/openrewrite/javascript/service/JavaScriptAutoFormatService.java); test relationship unresolved.

### org.openrewrite.javascript.style

Language style definitions and/or autodetection used by formatting; style values are separate from the visitor that applies them.

production; 6 source files. Surface: `Autodetect`, `PrettierStyle`, `SpacesStyle`, `TabsAndIndentsStyle`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/src/main/java/org/openrewrite/javascript/style/TabsAndIndentsStyle.java); [package annotations](../../rewrite-javascript/src/main/java/org/openrewrite/javascript/style/package-info.java); test relationship unresolved.

### org.openrewrite.javascript.table

Data-table schemas emitted by the module's recipes and analysis; actual row retention/output depends on the configured DataTableStore.

production; 3 source files. Surface: `NodeDependenciesInUse`, `NodeLockRegenerationFailures`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/src/main/java/org/openrewrite/javascript/table/NodeLockRegenerationFailures.java); [package annotations](../../rewrite-javascript/src/main/java/org/openrewrite/javascript/table/package-info.java); test relationship unresolved.

### org.openrewrite.javascript.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 8 source files. Surface: `JS`, `JSX`, `JsContainer`, `JsLeftPadded`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/src/main/java/org/openrewrite/javascript/tree/JsContainer.java); [package annotations](../../rewrite-javascript/src/main/java/org/openrewrite/javascript/tree/package-info.java); test relationship unresolved.

## rewrite-python

### rewrite

Python framework package and recipe activation entry point registered by pyproject.toml.

production; 16 source files. Surface: `__init__`, `category`, `data_table`, `decorators`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/rewrite/src/rewrite/__init__.py); test relationship unresolved.

### rewrite.java

Shared J-style tree/type, visitor and padding model for Python syntax and interop.

production; 6 source files. Surface: `__init__`, `extensions`, `markers`, `support_types`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/rewrite/src/rewrite/java/__init__.py); test relationship unresolved.

### rewrite.python

Python parser/tree/visitor/printer, import operations, type report and language-specific semantics.

production; 24 source files. Surface: `__init__`, `_parser_visitor`, `_py2_parser_visitor`, `_version_detect`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/rewrite/src/rewrite/python/__init__.py); test relationship unresolved.

### rewrite.python.format

Auto-format and conditional-format entry points plus whitespace/indentation visitors.

production; 11 source files. Surface: `__init__`, `_helpers`, `auto_format`, `blank_lines`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/rewrite/src/rewrite/python/format/__init__.py); test relationship unresolved.

### rewrite.python.recipes

Native Python recipe implementations exposed through the package activation path.

production; 9 source files. Surface: `__init__`, `add_literal_method_argument`, `change_import`, `change_method_name`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/rewrite/src/rewrite/python/recipes/__init__.py); test relationship unresolved.

### rewrite.python.search

Source-file, type and method precondition visitors for narrowing recipe work.

production; 1 source files. Surface: `__init__`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/rewrite/src/rewrite/python/search/__init__.py); test relationship unresolved.

### rewrite.python.template

Python coordinates, captures, pattern/template construction and replacement support.

production; 13 source files. Surface: `__init__`, `_fstring_support`, `_tstring_support`, `capture`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/rewrite/src/rewrite/python/template/__init__.py); test relationship unresolved.

### rewrite.rpc

Python RPC server/client, Java recipe bridge and aligned sender/receiver queues.

production; 14 source files. Surface: `__init__`, `__main__`, `bundle_children`, `child_connection`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/rewrite/src/rewrite/rpc/__init__.py); test relationship unresolved.

### rewrite.test

RecipeSpec/source specifications for before/after assertions and RPC-aware tests.

production; 4 source files. Surface: `__init__`, `rewrite_test`, `spec`, `well_formed`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/rewrite/src/rewrite/test/__init__.py); test relationship unresolved.

### org.openrewrite.python

PythonParser delegates language parsing to the native rewrite package. Java code additionally handles pyproject/Pipfile/requirements inputs, dependency metadata, PEP 440/508, index metadata and native lock regeneration for several formats. The Python distribution supplies tree/visitor/recipe machinery, parser/printer, templates and RPC. Its runtime requirement is Python >=3.12; parsing older Python syntax does not lower that interpreter requirement. Gradle creates/uses a local virtual environment and wires native pytest and Python-2 syntax compatibility separately.

production; 18 source files. Surface: `AddDependency`, `Assertions`, `ChangeDependency`, `DependencyWorkspace`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/PipfileParser.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/package-info.java); [test-source reference](../../rewrite-python/src/integTest/java/org/openrewrite/python/ChangeTypeIntegTest.java).

### org.openrewrite.python.format

Formatting recipes and visitors that adjust spacing, indentation or line breaks in the language model.

production; 3 source files. Surface: `PythonSpaces`, `PythonSpacesVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/format/PythonSpaces.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/format/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/format/PythonSpacesTest.java).

### org.openrewrite.python.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 19 source files. Surface: `CanonicalJson`, `Hashing`, `InstalledEnvParser`, `LockFileRegeneration`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/internal/InstalledEnvParser.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/internal/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/internal/PoetryLockParserTest.java).

### org.openrewrite.python.internal.index

Python package-index listings and artifact metadata used by native dependency/lock resolution.

production; 16 source files. Surface: `DistFilename`, `EnvExpansion`, `Environment`, `FlatIndexClient`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/internal/index/PackageListing.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/internal/index/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/internal/index/DistFilenameTest.java).

### org.openrewrite.python.internal.metadata

Python core-metadata parsing; MetadataParser stops header parsing at the first blank line and ignores the description body.

production; 6 source files. Surface: `CoreMetadata`, `LazyWheelMetadataReader`, `MetadataParser`, `Pep658MetadataFetcher`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/internal/metadata/MetadataParser.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/internal/metadata/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/internal/metadata/Pep658MetadataFetcherTest.java).

### org.openrewrite.python.internal.pdmlock

PDM lock-file data, parsing/writing and update engine; format exceptions reject unsupported shapes rather than silently discarding them.

production; 9 source files. Surface: `PdmContentHash`, `PdmLock`, `PdmLockEngine`, `PdmLockFile`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/internal/pdmlock/PdmLockFormatException.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/internal/pdmlock/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/internal/pdmlock/PdmLockRoundTripTest.java).

### org.openrewrite.python.internal.pep440

Python version/specifier semantics, including pre-release handling; source headers/Javadocs identify the ported pypa/packaging provenance.

production; 4 source files. Surface: `PythonVersion`, `PythonVersionSpecifier`, `PythonVersionSpecifierSet`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/internal/pep440/PythonVersionSpecifierSet.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/internal/pep440/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/internal/pep440/Corpus.java).

### org.openrewrite.python.internal.pep508

Python requirement and marker parsing; preserves written names/extras while also storing normalized package identity.

production; 5 source files. Surface: `Marker`, `MarkerEnvironment`, `Pep508Parser`, `Pep508Requirement`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/internal/pep508/Pep508Requirement.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/internal/pep508/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/internal/pep508/MarkerEnvironmentTest.java).

### org.openrewrite.python.internal.pipfilelock

Pipfile.lock structures and canonical JSON emission for pipenv-compatible content hashing.

production; 5 source files. Surface: `CanonicalJsonEmitter`, `PipenvLockEngine`, `PipfileLockHash`, `PipfileLockWriter`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/internal/pipfilelock/CanonicalJsonEmitter.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/internal/pipfilelock/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/internal/pipfilelock/Fixtures.java).

### org.openrewrite.python.internal.poetrylock

Poetry lock-file artifact/dependency data and regeneration support; fixture catalogs preserve exact format examples.

production; 13 source files. Surface: `PoetryConstraint`, `PoetryContentHash`, `PoetryLock`, `PoetryLockConstraint`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/internal/poetrylock/PoetryLockFile.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/internal/poetrylock/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/internal/poetrylock/PoetryLockRoundTripTest.java).

### org.openrewrite.python.internal.rpc

Tree-specific RPC codecs and send/receive visitors. The inspected codec delegates to matching sender/receiver implementations; their field order/type identities must stay aligned with the native peer.

production; 10 source files. Surface: `PythonContainerRpcCodec`, `PythonLeftPaddedRpcCodec`, `PythonReceiver`, `PythonRightPaddedRpcCodec`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/internal/rpc/PythonRpcCodec.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/internal/rpc/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/internal/rpc/PythonSenderReceiverRoundTripTest.java).

### org.openrewrite.python.internal.uvlock

uv.lock models, parsing/writing and delta-resolution engine, including workspace manifest fields; supported cases are bounded by format/behavior catalogs.

production; 22 source files. Surface: `EngineFailure`, `PyRange`, `UvLock`, `UvLockArtifact`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/internal/uvlock/UvLockManifest.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/internal/uvlock/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/internal/uvlock/UvLockRoundTripTest.java).

### org.openrewrite.python.marker

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 11 source files. Surface: `ExecSyntax`, `KeywordArguments`, `KeywordOnlyArguments`, `LegacyNotEqual`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/marker/SuppressNewline.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/marker/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/marker/PythonResolutionResultRpcTest.java).

### org.openrewrite.python.marketplace

Language-package recipe bundle discovery/resolution that bridges listings and preparation into the owning language runtime.

production; 2 source files. Surface: `PipRecipeBundleReader`, `PipRecipeBundleResolver`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/marketplace/PipRecipeBundleReader.java); test relationship unresolved.

### org.openrewrite.python.rpc

Language RPC façade, process configuration and request/response values for parse/install/visit operations; not every message type is public.

production; 11 source files. Surface: `Dependency`, `InstallRecipesByFile`, `InstallRecipesByPackage`, `InstallRecipesResponse`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/rpc/InstallRecipesByFile.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/rpc/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/rpc/PythonParseOptionsTest.java).

### org.openrewrite.python.search

Search/precondition recipes that select syntax or dependency findings; result markers/tables are distinct from rewriting matched source.

production; 3 source files. Surface: `DependencyInsight`, `UsesImport`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/search/UsesImport.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/search/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/search/FindTypesTest.java).

### org.openrewrite.python.service

Language-specific service adapters for comments, imports, naming, formatting or validation; inspect the concrete service's effects before calling it.

production; 6 source files. Surface: `PythonAddImportVisitor`, `PythonAutoFormatService`, `PythonImportService`, `PythonRemoveImportVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/service/PythonAutoFormatService.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/service/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/service/PythonRemoveImportVisitorTest.java).

### org.openrewrite.python.style

Language style definitions and/or autodetection used by formatting; style values are separate from the visitor that applies them.

production; 9 source files. Surface: `Autodetect`, `BlankLinesStyle`, `IntelliJ`, `OtherStyle`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/style/PythonStyle.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/style/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/style/AutodetectTest.java).

### org.openrewrite.python.table

Data-table schemas emitted by the module's recipes and analysis; actual row retention/output depends on the configured DataTableStore.

production; 3 source files. Surface: `PythonDependenciesInUse`, `PythonLockRegenerationFailures`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/table/PythonLockRegenerationFailures.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/table/package-info.java); test relationship unresolved.

### org.openrewrite.python.trait

Cursor-backed semantic views and matchers for language-specific constructs or references; use specific Matcher types where the Traits facade is deprecated.

production; 5 source files. Surface: `PipfileFile`, `PyProjectFile`, `PythonDependencyFile`, `RequirementsFile`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/trait/PythonDependencyFile.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/trait/package-info.java); [test-source reference](../../rewrite-python/src/test/java/org/openrewrite/python/trait/PipfileFileTest.java).

### org.openrewrite.python.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 7 source files. Surface: `Py`, `PyComment`, `PyContainer`, `PyLeftPadded`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/tree/PyComment.java); [package annotations](../../rewrite-python/src/main/java/org/openrewrite/python/tree/package-info.java); [test-source reference](../../rewrite-python/src/py2CompatibilityTest/java/org/openrewrite/python/tree/BacktickReprTest.java).

