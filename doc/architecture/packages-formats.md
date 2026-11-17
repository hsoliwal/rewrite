# Data and configuration formats: package responsibilities

[Architecture index](README.md) · [Module guide](modules.md) · [Coverage ledger](coverage.md)

Source baseline: `0c82f71bc8bb215a8194db59a3784551d6a60e99`. Entries describe source-directory groups and observed declarations. Java/Kotlin/Scala namespaces can span languages/artifacts; Go uses import paths, C# namespaces, Python import packages and TypeScript file modules. One representative declaration was read per group, so these are **partial responsibility summaries**, not an exhaustive namespace/member census. Listed surface names are examples present in the tree, not a claim that every type is public. Existing API documentation remains authoritative except where a specific drift is recorded.

Every entry retains its exact source directory and representative evidence in [coverage](coverage.md). The linked package-info files often contain only nullness declarations. All tests are source references, **not executed receipts**. Public-member parameters/returns/errors, side effects, lifecycle, dependency edges and individual Python/TypeScript file-module semantics still need full reconciliation; this common gap applies to every entry below.

## rewrite-docker

### org.openrewrite.docker

DockerParser uses generated ANTLR grammar and a parser visitor to create Docker trees. Recipes and traits inspect or change FROM/RUN/COPY and related instructions; search recipes emit image/port tables. ImageReferences is shared by parsing and traits to split name, tag and digest consistently. syncEolImages is a separate data-refresh task with an external-service effect, not a normal parser test.

production; 15 source files. Surface: `AddAptGetCleanup`, `AddOciLabels`, `AddOrUpdateLabel`, `AddUserInstruction`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-docker/src/main/java/org/openrewrite/docker/DockerParser.java); [package annotations](../../rewrite-docker/src/main/java/org/openrewrite/docker/package-info.java); [test-source reference](../../rewrite-docker/src/test/java/org/openrewrite/docker/DockerBestPracticesTest.java).

### org.openrewrite.docker.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 4 source files. Surface: `DockerParserVisitor`, `DockerPrinter`, `ImageReferences`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-docker/src/main/java/org/openrewrite/docker/internal/ImageReferences.java); [package annotations](../../rewrite-docker/src/main/java/org/openrewrite/docker/internal/package-info.java); [test-source reference](../../rewrite-docker/src/test/java/org/openrewrite/docker/internal/EolImageDataGenerator.java).

### org.openrewrite.docker.internal.grammar

Generated grammar/token/parser interfaces and automata, sometimes alongside handwritten base helpers. Parser visitors consume these structures; use the owning grammar/generation workflow and preserve handwritten helpers.

generated; 6 source files. Surface: `DockerLexer`, `DockerParser`, `DockerParserBaseListener`, `DockerParserBaseVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-docker/src/main/java/org/openrewrite/docker/internal/grammar/DockerParser.java); test relationship unresolved.

### org.openrewrite.docker.search

Search/precondition recipes that select syntax or dependency findings; result markers/tables are distinct from rewriting matched source.

production; 8 source files. Surface: `EolImage`, `FindBaseImages`, `FindEndOfLifeImages`, `FindExposedPorts`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-docker/src/main/java/org/openrewrite/docker/search/FindUnpinnedBaseImages.java); [package annotations](../../rewrite-docker/src/main/java/org/openrewrite/docker/search/package-info.java); [test-source reference](../../rewrite-docker/src/test/java/org/openrewrite/docker/search/FindMissingHealthcheckTest.java).

### org.openrewrite.docker.table

Data-table schemas emitted by the module's recipes and analysis; actual row retention/output depends on the configured DataTableStore.

production; 4 source files. Surface: `BaseImages`, `EolDockerImages`, `ExposedPorts`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-docker/src/main/java/org/openrewrite/docker/table/ExposedPorts.java); [package annotations](../../rewrite-docker/src/main/java/org/openrewrite/docker/table/package-info.java); test relationship unresolved.

### org.openrewrite.docker.trait

Cursor-backed semantic views and matchers for language-specific constructs or references; use specific Matcher types where the Traits facade is deprecated.

production; 4 source files. Surface: `DockerCopyFrom`, `DockerFrom`, `DockerImageReference`, `DockerTraitMatcher`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-docker/src/main/java/org/openrewrite/docker/trait/DockerTraitMatcher.java); [test-source reference](../../rewrite-docker/src/test/java/org/openrewrite/docker/trait/DockerImageReferenceTest.java).

### org.openrewrite.docker.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 4 source files. Surface: `Comment`, `Docker`, `Space`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-docker/src/main/java/org/openrewrite/docker/tree/Comment.java); [package annotations](../../rewrite-docker/src/main/java/org/openrewrite/docker/tree/package-info.java); [test-source reference](../../rewrite-docker/src/test/java/org/openrewrite/docker/tree/WorkdirTest.java).

## rewrite-hcl

### org.openrewrite.hcl

HclParser combines generated ANTLR syntax with Hcl trees and visitors. Search, style and template packages let recipes select and replace configuration content while retaining its formatting representation. generateAntlrSources consumes the checked-in grammars and writes generated Java; it is distinct from editing the handwritten parser visitor or printer.

production; 14 source files. Surface: `Assertions`, `CountLinesVisitor`, `DeleteContent`, `HclIsoVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-hcl/src/main/java/org/openrewrite/hcl/HclParser.java); [package annotations](../../rewrite-hcl/src/main/java/org/openrewrite/hcl/package-info.java); [test-source reference](../../rewrite-hcl/src/test/java/org/openrewrite/hcl/DeleteContentTest.java).

### org.openrewrite.hcl.format

Formatting recipes and visitors that adjust spacing, indentation or line breaks in the language model.

production; 16 source files. Surface: `AttributeSpaceVisitor`, `AutoFormat`, `AutoFormatVisitor`, `BlankLines`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-hcl/src/main/java/org/openrewrite/hcl/format/AutoFormat.java); [package annotations](../../rewrite-hcl/src/main/java/org/openrewrite/hcl/format/package-info.java); [test-source reference](../../rewrite-hcl/src/test/java/org/openrewrite/hcl/format/BracketSpacesTest.java).

### org.openrewrite.hcl.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 4 source files. Surface: `HclParserVisitor`, `HclPrinter`, `ThrowingErrorListener`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-hcl/src/main/java/org/openrewrite/hcl/internal/ThrowingErrorListener.java); [package annotations](../../rewrite-hcl/src/main/java/org/openrewrite/hcl/internal/package-info.java); test relationship unresolved.

### org.openrewrite.hcl.internal.grammar

Generated grammar/token/parser interfaces and automata, sometimes alongside handwritten base helpers. Parser visitors consume these structures; use the owning grammar/generation workflow and preserve handwritten helpers.

generated; 12 source files. Surface: `HCLLexer`, `HCLParser`, `HCLParserBaseListener`, `HCLParserBaseVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-hcl/src/main/java/org/openrewrite/hcl/internal/grammar/HCLParser.java); test relationship unresolved.

### org.openrewrite.hcl.internal.template

Internal snippet/template parsing, substitutions and generated context support; consumed by the language's public template layer.

production; 3 source files. Surface: `HclTemplateParser`, `Substitutions`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-hcl/src/main/java/org/openrewrite/hcl/internal/template/HclTemplateParser.java); [package annotations](../../rewrite-hcl/src/main/java/org/openrewrite/hcl/internal/template/package-info.java); test relationship unresolved.

### org.openrewrite.hcl.search

Search/precondition recipes that select syntax or dependency findings; result markers/tables are distinct from rewriting matched source.

production; 3 source files. Surface: `FindAndReplaceLiteral`, `FindContent`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-hcl/src/main/java/org/openrewrite/hcl/search/FindContent.java); [package annotations](../../rewrite-hcl/src/main/java/org/openrewrite/hcl/search/package-info.java); [test-source reference](../../rewrite-hcl/src/test/java/org/openrewrite/hcl/search/FindContentTest.java).

### org.openrewrite.hcl.style

Language style definitions and/or autodetection used by formatting; style values are separate from the visitor that applies them.

production; 4 source files. Surface: `BracketsStyle`, `SpacesStyle`, `TabsAndIndentsStyle`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-hcl/src/main/java/org/openrewrite/hcl/style/SpacesStyle.java); [package annotations](../../rewrite-hcl/src/main/java/org/openrewrite/hcl/style/package-info.java); test relationship unresolved.

### org.openrewrite.hcl.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 13 source files. Surface: `BodyContent`, `CollectionValue`, `Comment`, `CoordinateBuilder`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-hcl/src/main/java/org/openrewrite/hcl/tree/Label.java); [package annotations](../../rewrite-hcl/src/main/java/org/openrewrite/hcl/tree/package-info.java); [test-source reference](../../rewrite-hcl/src/test/java/org/openrewrite/hcl/tree/HclIndexTest.java).

## rewrite-json

### org.openrewrite.json

JsonParser and JsonVisitor work with Json documents, keys, values and spacing/comment structures. JSON-path grammar supports selection; formatting, search, comment services and RPC codecs sit above the tree layer. JsonCommentService handles comments stored in node prefixes. Read parser acceptance and source tests for actual supported syntax rather than assuming every JSON-like dialect is accepted.

production; 15 source files. Surface: `AddKeyValue`, `Assertions`, `ChangeKey`, `ChangeValue`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-json/src/main/java/org/openrewrite/json/JsonParser.java); [package annotations](../../rewrite-json/src/main/java/org/openrewrite/json/package-info.java); [test-source reference](../../rewrite-json/src/test/java/org/openrewrite/json/DeleteKeyTest.java).

### org.openrewrite.json.format

Formatting recipes and visitors that adjust spacing, indentation or line breaks in the language model.

production; 8 source files. Surface: `AutoFormat`, `AutoFormatVisitor`, `Indents`, `NormalizeLineBreaksVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-json/src/main/java/org/openrewrite/json/format/AutoFormat.java); [package annotations](../../rewrite-json/src/main/java/org/openrewrite/json/format/package-info.java); [test-source reference](../../rewrite-json/src/test/java/org/openrewrite/json/format/NormalizeLineBreaksVisitorTest.java).

### org.openrewrite.json.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 3 source files. Surface: `JsonParserVisitor`, `JsonPrinter`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-json/src/main/java/org/openrewrite/json/internal/JsonPrinter.java); [package annotations](../../rewrite-json/src/main/java/org/openrewrite/json/internal/package-info.java); test relationship unresolved.

### org.openrewrite.json.internal.grammar

Generated grammar/token/parser interfaces and automata, sometimes alongside handwritten base helpers. Parser visitors consume these structures; use the owning grammar/generation workflow and preserve handwritten helpers.

generated; 12 source files. Surface: `JSON5BaseListener`, `JSON5BaseVisitor`, `JSON5Lexer`, `JSON5Listener`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-json/src/main/java/org/openrewrite/json/internal/grammar/JsonPathParser.java); test relationship unresolved.

### org.openrewrite.json.internal.rpc

Tree-specific RPC codecs and send/receive visitors. The inspected codec delegates to matching sender/receiver implementations; their field order/type identities must stay aligned with the native peer.

production; 4 source files. Surface: `JsonReceiver`, `JsonRpcCodec`, `JsonSender`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-json/src/main/java/org/openrewrite/json/internal/rpc/JsonRpcCodec.java); [package annotations](../../rewrite-json/src/main/java/org/openrewrite/json/internal/rpc/package-info.java); [test-source reference](../../rewrite-json/src/test/java/org/openrewrite/json/internal/rpc/package-info.java).

### org.openrewrite.json.marker

Namespace/nullness declaration only at this revision; no marker implementation in this source directory.

stub; 1 source files. Surface: declaration-only. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-json/src/main/java/org/openrewrite/json/marker/package-info.java); [package annotations](../../rewrite-json/src/main/java/org/openrewrite/json/marker/package-info.java); test relationship unresolved.

### org.openrewrite.json.search

Search/precondition recipes that select syntax or dependency findings; result markers/tables are distinct from rewriting matched source.

production; 2 source files. Surface: `FindKey`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-json/src/main/java/org/openrewrite/json/search/FindKey.java); [package annotations](../../rewrite-json/src/main/java/org/openrewrite/json/search/package-info.java); [test-source reference](../../rewrite-json/src/test/java/org/openrewrite/json/search/FindKeyTest.java).

### org.openrewrite.json.service

Language-specific service adapters for comments, imports, naming, formatting or validation; inspect the concrete service's effects before calling it.

production; 2 source files. Surface: `JsonCommentService`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-json/src/main/java/org/openrewrite/json/service/JsonCommentService.java); [package annotations](../../rewrite-json/src/main/java/org/openrewrite/json/service/package-info.java); test relationship unresolved.

### org.openrewrite.json.style

Language style definitions and/or autodetection used by formatting; style values are separate from the visitor that applies them.

production; 5 source files. Surface: `Autodetect`, `JsonStyle`, `TabsAndIndentsStyle`, `WrappingAndBracesStyle`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-json/src/main/java/org/openrewrite/json/style/JsonStyle.java); [package annotations](../../rewrite-json/src/main/java/org/openrewrite/json/style/package-info.java); [test-source reference](../../rewrite-json/src/test/java/org/openrewrite/json/style/AutodetectTest.java).

### org.openrewrite.json.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 7 source files. Surface: `Comment`, `Json`, `JsonKey`, `JsonRightPadded`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-json/src/main/java/org/openrewrite/json/tree/JsonKey.java); [package annotations](../../rewrite-json/src/main/java/org/openrewrite/json/tree/package-info.java); test relationship unresolved.

## rewrite-properties

### org.openrewrite.properties

PropertiesParser produces Properties.File entries and comment nodes with continuation/format information. Recipes find/change keys and values; PropertiesReference exposes reference traits. PropertiesCommentService treats comments as sibling nodes, unlike the prefix-based comments in JSON/YAML. It depends on core without a javac parser requirement.

production; 13 source files. Surface: `AddProperty`, `AddPropertyComment`, `Assertions`, `ChangePropertyKey`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-properties/src/main/java/org/openrewrite/properties/PropertiesParser.java); [package annotations](../../rewrite-properties/src/main/java/org/openrewrite/properties/package-info.java); [test-source reference](../../rewrite-properties/src/test/java/org/openrewrite/properties/Issue1020Test.java).

### org.openrewrite.properties.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 2 source files. Surface: `PropertiesPrinter`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-properties/src/main/java/org/openrewrite/properties/internal/PropertiesPrinter.java); [package annotations](../../rewrite-properties/src/main/java/org/openrewrite/properties/internal/package-info.java); test relationship unresolved.

### org.openrewrite.properties.search

Search/precondition recipes that select syntax or dependency findings; result markers/tables are distinct from rewriting matched source.

production; 2 source files. Surface: `FindProperties`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-properties/src/main/java/org/openrewrite/properties/search/FindProperties.java); [package annotations](../../rewrite-properties/src/main/java/org/openrewrite/properties/search/package-info.java); [test-source reference](../../rewrite-properties/src/test/java/org/openrewrite/properties/search/FindPropertiesTest.java).

### org.openrewrite.properties.service

Language-specific service adapters for comments, imports, naming, formatting or validation; inspect the concrete service's effects before calling it.

production; 2 source files. Surface: `PropertiesCommentService`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-properties/src/main/java/org/openrewrite/properties/service/PropertiesCommentService.java); [package annotations](../../rewrite-properties/src/main/java/org/openrewrite/properties/service/package-info.java); test relationship unresolved.

### org.openrewrite.properties.trait

Cursor-backed semantic views and matchers for language-specific constructs or references; use specific Matcher types where the Traits facade is deprecated.

production; 2 source files. Surface: `PropertiesReference`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-properties/src/main/java/org/openrewrite/properties/trait/PropertiesReference.java); [package annotations](../../rewrite-properties/src/main/java/org/openrewrite/properties/trait/package-info.java); [test-source reference](../../rewrite-properties/src/test/java/org/openrewrite/properties/trait/PropertiesReferenceTest.java).

### org.openrewrite.properties.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 2 source files. Surface: `Properties`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-properties/src/main/java/org/openrewrite/properties/tree/Properties.java); [package annotations](../../rewrite-properties/src/main/java/org/openrewrite/properties/tree/package-info.java); test relationship unresolved.

## rewrite-protobuf

### org.openrewrite.protobuf

ProtoParser, ProtoVisitor and Proto trees represent schema text rather than generated message implementations. The generated grammar is named Protobuf2; ImplicitProto2Syntax records an omitted syntax declaration. Do not infer universal proto3/edition support from the module name. Generated grammar code, handwritten parse mapping, markers and printer have separate responsibilities.

production; 7 source files. Surface: `Assertions`, `CountLinesVisitor`, `ProtoIsoVisitor`, `ProtoParser`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-protobuf/src/main/java/org/openrewrite/protobuf/ProtoParser.java); [package annotations](../../rewrite-protobuf/src/main/java/org/openrewrite/protobuf/package-info.java); [test-source reference](../../rewrite-protobuf/src/test/java/org/openrewrite/protobuf/ProtoParserTest.java).

### org.openrewrite.protobuf.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 3 source files. Surface: `ProtoParserVisitor`, `ProtoPrinter`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-protobuf/src/main/java/org/openrewrite/protobuf/internal/ProtoPrinter.java); [package annotations](../../rewrite-protobuf/src/main/java/org/openrewrite/protobuf/internal/package-info.java); test relationship unresolved.

### org.openrewrite.protobuf.internal.grammar

Generated grammar/token/parser interfaces and automata, sometimes alongside handwritten base helpers. Parser visitors consume these structures; use the owning grammar/generation workflow and preserve handwritten helpers.

generated; 6 source files. Surface: `Protobuf2Lexer`, `Protobuf2Parser`, `Protobuf2ParserBaseListener`, `Protobuf2ParserBaseVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-protobuf/src/main/java/org/openrewrite/protobuf/internal/grammar/Protobuf2ParserVisitor.java); test relationship unresolved.

### org.openrewrite.protobuf.marker

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 2 source files. Surface: `ImplicitProto2Syntax`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-protobuf/src/main/java/org/openrewrite/protobuf/marker/ImplicitProto2Syntax.java); [package annotations](../../rewrite-protobuf/src/main/java/org/openrewrite/protobuf/marker/package-info.java); test relationship unresolved.

### org.openrewrite.protobuf.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 10 source files. Surface: `Comment`, `FullName`, `Proto`, `ProtoContainer`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-protobuf/src/main/java/org/openrewrite/protobuf/tree/FullName.java); [package annotations](../../rewrite-protobuf/src/main/java/org/openrewrite/protobuf/tree/package-info.java); [test-source reference](../../rewrite-protobuf/src/test/java/org/openrewrite/protobuf/tree/SyntaxTest.java).

## rewrite-toml

### org.openrewrite.toml

TomlParser uses ANTLR and a handwritten mapping to Toml keys, values, tables and spacing. Recipes change/delete keys, table values and rows or create files. ArrayTable and InlineTable preserve syntax distinctions through markers; the printer reconstructs source from these shapes. It is reused by Gradle version catalogs and Python dependency-file support.

production; 20 source files. Surface: `Assertions`, `ChangeKey`, `ChangeTableRowValue`, `ChangeValue`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-toml/src/main/java/org/openrewrite/toml/TomlParser.java); [package annotations](../../rewrite-toml/src/main/java/org/openrewrite/toml/package-info.java); [test-source reference](../../rewrite-toml/src/test/java/org/openrewrite/toml/TomlVisitorTest.java).

### org.openrewrite.toml.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 3 source files. Surface: `TomlParserVisitor`, `TomlPrinter`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-toml/src/main/java/org/openrewrite/toml/internal/TomlPrinter.java); [package annotations](../../rewrite-toml/src/main/java/org/openrewrite/toml/internal/package-info.java); test relationship unresolved.

### org.openrewrite.toml.internal.grammar

Generated grammar/token/parser interfaces and automata, sometimes alongside handwritten base helpers. Parser visitors consume these structures; use the owning grammar/generation workflow and preserve handwritten helpers.

generated; 6 source files. Surface: `TomlLexer`, `TomlParser`, `TomlParserBaseListener`, `TomlParserBaseVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-toml/src/main/java/org/openrewrite/toml/internal/grammar/TomlParser.java); test relationship unresolved.

### org.openrewrite.toml.marker

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 3 source files. Surface: `ArrayTable`, `InlineTable`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-toml/src/main/java/org/openrewrite/toml/marker/ArrayTable.java); [package annotations](../../rewrite-toml/src/main/java/org/openrewrite/toml/marker/package-info.java); test relationship unresolved.

### org.openrewrite.toml.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 8 source files. Surface: `Comment`, `Space`, `Toml`, `TomlKey`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-toml/src/main/java/org/openrewrite/toml/tree/TomlKey.java); [package annotations](../../rewrite-toml/src/main/java/org/openrewrite/toml/tree/package-info.java); test relationship unresolved.

## rewrite-xml

### org.openrewrite.xml

XmlParser maps XML grammar output into Xml source trees used directly and by Maven/C# project support. Search and recipe helpers operate on tags, attributes and namespaces. Comments are explicit Xml.Comment nodes. HTML-like void-element tolerance is path-sensitive in the parser and recorded with HtmlVoidElement. The build can generate both Java XML grammar code and C# grammar code in the sibling C# SDK.

production; 32 source files. Surface: `AddCommentToXmlTag`, `AddOrUpdateChild`, `AddOrUpdateChildTag`, `AddTagAttribute`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/src/main/java/org/openrewrite/xml/XmlParser.java); [package annotations](../../rewrite-xml/src/main/java/org/openrewrite/xml/package-info.java); [test-source reference](../../rewrite-xml/src/test/java/org/openrewrite/xml/ChangeTagNameTest.java).

### org.openrewrite.xml.format

Formatting recipes and visitors that adjust spacing, indentation or line breaks in the language model.

production; 16 source files. Surface: `AutoFormat`, `AutoFormatVisitor`, `AutodetectGeneralFormatStyle`, `LineBreaks`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/src/main/java/org/openrewrite/xml/format/LineBreaks.java); [package annotations](../../rewrite-xml/src/main/java/org/openrewrite/xml/format/package-info.java); [test-source reference](../../rewrite-xml/src/test/java/org/openrewrite/xml/format/LineBreaksTest.java).

### org.openrewrite.xml.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 6 source files. Surface: `ThrowingErrorListener`, `WithPrefix`, `XmlParserVisitor`, `XmlPrinter`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/src/main/java/org/openrewrite/xml/internal/WithPrefix.java); [package annotations](../../rewrite-xml/src/main/java/org/openrewrite/xml/internal/package-info.java); test relationship unresolved.

### org.openrewrite.xml.internal.grammar

Generated grammar/token/parser interfaces and automata, sometimes alongside handwritten base helpers. Parser visitors consume these structures; use the owning grammar/generation workflow and preserve handwritten helpers.

generated; 13 source files. Surface: `XMLLexer`, `XMLParser`, `XMLParserBase`, `XMLParserBaseListener`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/src/main/java/org/openrewrite/xml/internal/grammar/XMLParser.java); test relationship unresolved.

### org.openrewrite.xml.internal.rpc

Tree-specific RPC codecs and send/receive visitors. The inspected codec delegates to matching sender/receiver implementations; their field order/type identities must stay aligned with the native peer.

production; 3 source files. Surface: `XmlReceiver`, `XmlRpcCodec`, `XmlSender`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/src/main/java/org/openrewrite/xml/internal/rpc/XmlRpcCodec.java); [test-source reference](../../rewrite-xml/src/test/java/org/openrewrite/xml/internal/rpc/XmlSendReceiveTest.java).

### org.openrewrite.xml.marker

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 1 source files. Surface: `HtmlVoidElement`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/src/main/java/org/openrewrite/xml/marker/HtmlVoidElement.java); test relationship unresolved.

### org.openrewrite.xml.search

Search/precondition recipes that select syntax or dependency findings; result markers/tables are distinct from rewriting matched source.

production; 5 source files. Surface: `DoesNotUseNamespaceUri`, `FindNamespacePrefix`, `FindTags`, `HasNamespaceUri`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/src/main/java/org/openrewrite/xml/search/DoesNotUseNamespaceUri.java); [package annotations](../../rewrite-xml/src/main/java/org/openrewrite/xml/search/package-info.java); [test-source reference](../../rewrite-xml/src/test/java/org/openrewrite/xml/search/DoesNotUseNamespaceUriTest.java).

### org.openrewrite.xml.security

Recipes for repository/suppression-file security configuration. The recipe name is not a general security-audit guarantee.

production; 5 source files. Surface: `AddOwaspDateBoundSuppressions`, `IsOwaspSuppressionsFile`, `RemoveOwaspSuppressions`, `UpdateOwaspSuppressionDate`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/src/main/java/org/openrewrite/xml/security/IsOwaspSuppressionsFile.java); [package annotations](../../rewrite-xml/src/main/java/org/openrewrite/xml/security/package-info.java); [test-source reference](../../rewrite-xml/src/test/java/org/openrewrite/xml/security/AddOwaspDateBoundSuppressionsTest.java).

### org.openrewrite.xml.service

Language-specific service adapters for comments, imports, naming, formatting or validation; inspect the concrete service's effects before calling it.

production; 2 source files. Surface: `XmlCommentService`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/src/main/java/org/openrewrite/xml/service/XmlCommentService.java); [package annotations](../../rewrite-xml/src/main/java/org/openrewrite/xml/service/package-info.java); test relationship unresolved.

### org.openrewrite.xml.style

Language style definitions and/or autodetection used by formatting; style values are separate from the visitor that applies them.

production; 5 source files. Surface: `Autodetect`, `AutodetectDebug`, `TabsAndIndentsStyle`, `XmlStyle`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/src/main/java/org/openrewrite/xml/style/XmlStyle.java); [package annotations](../../rewrite-xml/src/main/java/org/openrewrite/xml/style/package-info.java); test relationship unresolved.

### org.openrewrite.xml.table

Data-table schemas emitted by the module's recipes and analysis; actual row retention/output depends on the configured DataTableStore.

production; 1 source files. Surface: `XmlStyleReport`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/src/main/java/org/openrewrite/xml/table/XmlStyleReport.java); test relationship unresolved.

### org.openrewrite.xml.trait

Cursor-backed semantic views and matchers for language-specific constructs or references; use specific Matcher types where the Traits facade is deprecated.

production; 4 source files. Surface: `Namespaced`, `SpringXmlReference`, `XmlReference`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/src/main/java/org/openrewrite/xml/trait/XmlReference.java); [package annotations](../../rewrite-xml/src/main/java/org/openrewrite/xml/trait/package-info.java); [test-source reference](../../rewrite-xml/src/test/java/org/openrewrite/xml/trait/NamespacedTest.java).

### org.openrewrite.xml.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 4 source files. Surface: `Content`, `Misc`, `Xml`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/src/main/java/org/openrewrite/xml/tree/Misc.java); [package annotations](../../rewrite-xml/src/main/java/org/openrewrite/xml/tree/package-info.java); test relationship unresolved.

## rewrite-yaml

### org.openrewrite.yaml

YamlParser uses SnakeYAML events and source tracking to build documents, mappings, sequences, scalars and aliases. JsonPath grammar is for selection rather than the YAML parser itself. Formatting and comment services operate on prefix strings; YamlKey permits scalar or alias blocks. Recipes modify keys/values, merge documents and clean up unused syntax.

production; 30 source files. Surface: `AddCommentToProperty`, `AppendToSequence`, `AppendToSequenceVisitor`, `Assertions`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml/src/main/java/org/openrewrite/yaml/YamlParser.java); [package annotations](../../rewrite-yaml/src/main/java/org/openrewrite/yaml/package-info.java); [test-source reference](../../rewrite-yaml/src/test/java/org/openrewrite/yaml/CountLinesVisitorTest.java).

### org.openrewrite.yaml.cleanup

Focused cleanup transformations for this language's syntax; each Recipe owns its own preconditions and before/after semantics.

production; 3 source files. Surface: `RemoveUnused`, `RemoveUnusedVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml/src/main/java/org/openrewrite/yaml/cleanup/RemoveUnused.java); [package annotations](../../rewrite-yaml/src/main/java/org/openrewrite/yaml/cleanup/package-info.java); [test-source reference](../../rewrite-yaml/src/test/java/org/openrewrite/yaml/cleanup/RemoveUnusedVisitorTest.java).

### org.openrewrite.yaml.format

Formatting recipes and visitors that adjust spacing, indentation or line breaks in the language model.

production; 7 source files. Surface: `AutoFormatVisitor`, `Indents`, `IndentsVisitor`, `MinimumViableSpacingVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml/src/main/java/org/openrewrite/yaml/format/Indents.java); [package annotations](../../rewrite-yaml/src/main/java/org/openrewrite/yaml/format/package-info.java); [test-source reference](../../rewrite-yaml/src/test/java/org/openrewrite/yaml/format/MinimumViableSpacingVisitorTest.java).

### org.openrewrite.yaml.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 4 source files. Surface: `StringUtils`, `ThrowingErrorListener`, `YamlPrinter`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml/src/main/java/org/openrewrite/yaml/internal/ThrowingErrorListener.java); [package annotations](../../rewrite-yaml/src/main/java/org/openrewrite/yaml/internal/package-info.java); [test-source reference](../../rewrite-yaml/src/test/java/org/openrewrite/yaml/internal/StringUtilsTest.java).

### org.openrewrite.yaml.internal.grammar

Generated grammar/token/parser interfaces and automata, sometimes alongside handwritten base helpers. Parser visitors consume these structures; use the owning grammar/generation workflow and preserve handwritten helpers.

generated; 6 source files. Surface: `JsonPathLexer`, `JsonPathParser`, `JsonPathParserBaseListener`, `JsonPathParserBaseVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml/src/main/java/org/openrewrite/yaml/internal/grammar/JsonPathParser.java); test relationship unresolved.

### org.openrewrite.yaml.internal.rpc

Tree-specific RPC codecs and send/receive visitors. The inspected codec delegates to matching sender/receiver implementations; their field order/type identities must stay aligned with the native peer.

production; 4 source files. Surface: `YamlReceiver`, `YamlRpcCodec`, `YamlSender`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml/src/main/java/org/openrewrite/yaml/internal/rpc/YamlRpcCodec.java); [package annotations](../../rewrite-yaml/src/main/java/org/openrewrite/yaml/internal/rpc/package-info.java); test relationship unresolved.

### org.openrewrite.yaml.marker

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 1 source files. Surface: `OmitColon`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml/src/main/java/org/openrewrite/yaml/marker/OmitColon.java); test relationship unresolved.

### org.openrewrite.yaml.search

Search/precondition recipes that select syntax or dependency findings; result markers/tables are distinct from rewriting matched source.

production; 4 source files. Surface: `FindIndentYamlVisitor`, `FindKey`, `FindProperty`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml/src/main/java/org/openrewrite/yaml/search/FindKey.java); [package annotations](../../rewrite-yaml/src/main/java/org/openrewrite/yaml/search/package-info.java); [test-source reference](../../rewrite-yaml/src/test/java/org/openrewrite/yaml/search/FindKeyTest.java).

### org.openrewrite.yaml.service

Language-specific service adapters for comments, imports, naming, formatting or validation; inspect the concrete service's effects before calling it.

production; 2 source files. Surface: `YamlCommentService`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml/src/main/java/org/openrewrite/yaml/service/YamlCommentService.java); [package annotations](../../rewrite-yaml/src/main/java/org/openrewrite/yaml/service/package-info.java); test relationship unresolved.

### org.openrewrite.yaml.style

Language style definitions and/or autodetection used by formatting; style values are separate from the visitor that applies them.

production; 5 source files. Surface: `Autodetect`, `IndentsStyle`, `YamlDefaultStyles`, `YamlStyle`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml/src/main/java/org/openrewrite/yaml/style/YamlStyle.java); [package annotations](../../rewrite-yaml/src/main/java/org/openrewrite/yaml/style/package-info.java); [test-source reference](../../rewrite-yaml/src/test/java/org/openrewrite/yaml/style/AutodetectTest.java).

### org.openrewrite.yaml.trait

Cursor-backed semantic views and matchers for language-specific constructs or references; use specific Matcher types where the Traits facade is deprecated.

production; 5 source files. Surface: `BlockScalar`, `YamlApplicationConfigReference`, `YamlReference`, `YamlValue`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml/src/main/java/org/openrewrite/yaml/trait/YamlReference.java); [package annotations](../../rewrite-yaml/src/main/java/org/openrewrite/yaml/trait/package-info.java); [test-source reference](../../rewrite-yaml/src/test/java/org/openrewrite/yaml/trait/YamlValueTest.java).

### org.openrewrite.yaml.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 3 source files. Surface: `Yaml`, `YamlKey`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml/src/main/java/org/openrewrite/yaml/tree/YamlKey.java); [package annotations](../../rewrite-yaml/src/main/java/org/openrewrite/yaml/tree/package-info.java); [test-source reference](../../rewrite-yaml/src/test/java/org/openrewrite/yaml/tree/ArrayTest.java).

