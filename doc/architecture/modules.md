# Projects and modules

[Architecture index](README.md) · [Coverage](coverage.md)

Source baseline: `0c82f71bc8bb215a8194db59a3784551d6a60e99`. All commands below are source-derived instructions, **not executed receipts**. Run from the repository root unless another directory is stated. Read [CONTRIBUTING.md](../../CONTRIBUTING.md) first: dynamic dependencies, plugin resolution and the selected JDK/native toolchains can block configuration before any task runs. Building or testing may download dependencies, create environments, launch subprocesses or write generated outputs.

## Explicit Gradle projects

Each section covers both the Gradle project boundary and its principal module. A project's main artifact is a JVM library unless identified as a platform or benchmark module. Exact publication variants and inherited tasks remain owned by the shared build plugins; no publication was performed. Dependency lists below preserve the configuration names from the inspected build files, rather than flattening test dependencies into runtime requirements.

## rewrite-android

**Cross-language Android build/source helpers.** Recipes update Android Gradle plugin and SDK declarations across Gradle scripts, properties and version catalogs. Android markers describe project variants and SDK metadata; traits recognize Android-relevant Java/Kotlin syntax. There is no AndroidVisitor: package-level documentation explicitly composes traits with visitors/scanning recipes across source languages and XML resources.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-android/src/main/java/org/openrewrite/android/trait/package-info.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-android/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-android)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-android); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-android/src/test](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-android/src/test); suggested focused entry: `./gradlew :rewrite-android:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `api(project(":rewrite-gradle"))`; `api(project(":rewrite-java"))`; `api(project(":rewrite-kotlin"))`; `api(project(":rewrite-xml"))`; `implementation(project(":rewrite-properties"))`; `implementation(project(":rewrite-toml"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`; `testImplementation(project(":rewrite-java-tck"))`; `testRuntimeOnly(project(":rewrite-java-21"))`

## rewrite-benchmarks

**JMH performance experiments.** Contains core, Java, Maven and recipe-loading benchmark packages under src/jmh/java. The build configures JMH forks/iterations and optional jmh.includes, and writes a resolved recipe-module classpath before jmh. Some benchmarks parse dependency-bearing POMs or use persistent caches. No throughput, memory or regression result is established by merely reading these sources.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-benchmarks/README.md); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-benchmarks/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-benchmarks)
- Packages/source sets: [owned package guide](packages-core.md#rewrite-benchmarks); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-benchmarks/src/jmh/java/org/openrewrite/benchmarks/recipe/RecipeLoadingBenchmark.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-benchmarks/src/jmh/java/org/openrewrite/benchmarks/recipe/RecipeLoadingBenchmark.java); suggested focused entry: `./gradlew :rewrite-benchmarks:jmh -Pjmh.includes=RecipeLoadingBenchmark`
- Internal dependency declarations: `jmh(project(":rewrite-core"))`; `jmh(project(":rewrite-java"))`; `jmh(project(":rewrite-java-21"))`; `jmh(project(":rewrite-javascript"))`; `jmh(project(":rewrite-maven"))`

## rewrite-bom

**Published dependency-alignment platform.** Uses java-platform and build publishing/metadata plugins to emit constraints over selected root subprojects. The filter excludes this project, benchmark/TCK names, tools and the tooling-model parent name. It has no source package or parser; its public surface is dependency metadata. The exact constraints depend on the projects included by settings.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-bom/build.gradle.kts); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-bom/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-bom)
- Packages/source sets: [owned package guide](packages-core.md#rewrite-bom); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-bom/build.gradle.kts](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-bom/build.gradle.kts); suggested focused entry: `./gradlew :rewrite-bom:assemble`
- Internal dependency declarations: no direct static project dependency statement in this manifest; see its external dependencies or dynamic platform constraints

## rewrite-core

**Language-independent transformation engine.** Owns Parser, SourceFile/Tree, TreeVisitor, Recipe, execution context, markers, tables, scheduling, recipe discovery and RPC infrastructure. Parsers supply source trees; recipes return visitors or compose child recipes. A returned tree is the result of the visit, while ExecutionContext carries mutable messages and errors. This module also provides plain-text, binary and quark representations, so callers need not force every file through a language parser.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/main/java/org/openrewrite/Parser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core)
- Packages/source sets: [owned package guide](packages-core.md#rewrite-core); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-core/src/test/java/org/openrewrite/RecipeRunTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-core/src/test/java/org/openrewrite/RecipeRunTest.java); suggested focused entry: `./gradlew :rewrite-core:test`
- Internal dependency declarations: `testImplementation(project(":rewrite-test"))`

## rewrite-csharp

**Java host for C# parsing and recipes.** Provides CSharpParser, Cs/Linq trees, Java-side RPC codecs, MSBuild path recipes, styles and service adapters. CSharpParser delegates solution/project parsing to a .NET process. The nested SDK, tool and xUnit projects implement the native side with Roslyn and StreamJsonRpc. csharpBuild, csharpTest and integTest are distinct stages; check includes both native tests and Java-to-C# integration tests.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/src/main/java/org/openrewrite/csharp/CSharpParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp)
- Packages/source sets: [owned package guide](packages-polyglot.md#rewrite-csharp); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-csharp/src/integTest](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/src/integTest); suggested focused entry: `./gradlew :rewrite-csharp:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `api(project(":rewrite-java"))`; `compileOnly(project(":rewrite-test"))`; `compileOnly(project(":rewrite-xml"))`; `testImplementation(project(":rewrite-test"))`; `testImplementation(project(":rewrite-xml"))`; `testImplementation(project(":rewrite-maven"))`; `testRuntimeOnly(project(":rewrite-java-21"))`; `implementation(project())`; `implementation(project(":rewrite-java-21"))`; `implementation(project(":rewrite-test"))`; `implementation(project(":rewrite-xml"))`

## rewrite-docker

**Dockerfile trees, recipes and image inspection.** DockerParser uses generated ANTLR grammar and a parser visitor to create Docker trees. Recipes and traits inspect or change FROM/RUN/COPY and related instructions; search recipes emit image/port tables. ImageReferences is shared by parsing and traits to split name, tag and digest consistently. syncEolImages is a separate data-refresh task with an external-service effect, not a normal parser test.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-docker/src/main/java/org/openrewrite/docker/DockerParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-docker/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-docker)
- Packages/source sets: [owned package guide](packages-formats.md#rewrite-docker); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-docker/src/test/java/org/openrewrite/docker/DockerParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-docker/src/test/java/org/openrewrite/docker/DockerParserTest.java); suggested focused entry: `./gradlew :rewrite-docker:test`
- Internal dependency declarations: `implementation(project(":rewrite-core"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`

## rewrite-go

**Java host and Go-native transformation runtime.** Java GolangParser/GoModParser/GoSumParser and Go/J tree/codecs bridge to the Go module in this directory. cmd/rpc is the native RPC entry point; pkg/parser, tree, visitor, printer, recipe, template and rpc implement its language runtime. The Gradle build stages Go source and vendored dependencies as resources, builds an RPC executable, runs goTest and defines a separate integTest suite. GoModParser currently returns GoMod through RPC despite its older PlainText class comment.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/src/main/java/org/openrewrite/golang/GoModParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go)
- Packages/source sets: [owned package guide](packages-polyglot.md#rewrite-go); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-go/src/integTest/java/org/openrewrite/golang/GoModParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/src/integTest/java/org/openrewrite/golang/GoModParserTest.java); suggested focused entry: `./gradlew :rewrite-go:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `api(project(":rewrite-java"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`; `testRuntimeOnly(project(":rewrite-java-21"))`; `implementation(project())`; `implementation(project(":rewrite-java-21"))`; `implementation(project(":rewrite-test"))`

## rewrite-gradle

**Gradle Groovy/Kotlin build-file recipes.** GradleParser delegates script syntax to Groovy/Kotlin parsers and attaches Gradle-specific model context. Recipes operate on dependencies, plugins, wrappers, repositories and version catalogs; marker/attribute packages represent extracted build semantics. This is the recipe/model library, not the separately distributed OpenRewrite Gradle plugin. The two tooling-model projects support extraction/testing of Gradle semantics.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/GradleParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-gradle); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-gradle/src/test/java/org/openrewrite/gradle/GradleParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/test/java/org/openrewrite/gradle/GradleParserTest.java); suggested focused entry: `./gradlew :rewrite-gradle:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `api(project(":rewrite-groovy"))`; `api(project(":rewrite-kotlin"))`; `api(project(":rewrite-maven"))`; `api(project(":rewrite-toml"))`; `implementation(project(":rewrite-properties"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`; `testImplementation(project(":rewrite-gradle-tooling-model:model"))`; `testRuntimeOnly(project(":rewrite-java-21"))`

## rewrite-gradle-tooling-model:model

**Serializable Gradle extraction model.** Defines OpenRewriteModel and its model/proxy/implementation shapes used to extract information from Gradle builds for refactoring tests. The build uses compileOnly dependencies on language/model libraries and provides a pluginLocalTestClasspath configuration. It includes a Java 8-launched testGradle4 compatibility task alongside the ordinary Java 21 setup.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle-tooling-model/model/src/main/java/org/openrewrite/gradle/toolingapi/OpenRewriteModelImpl.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle-tooling-model/model/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle-tooling-model/model)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-gradle-tooling-modelmodel); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-gradle-tooling-model/model/src/test](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle-tooling-model/model/src/test); suggested focused entry: `./gradlew :rewrite-gradle-tooling-model:model:test`
- Internal dependency declarations: `compileOnly(project(":rewrite-core"))`; `compileOnly(project(":rewrite-maven"))`; `compileOnly(project(":rewrite-test"))`; `compileOnly(project(":rewrite-gradle"))`; `compileOnly(project(":rewrite-groovy"))`; `compileOnly(project(":rewrite-kotlin"))`; `compileOnly(project(":rewrite-properties"))`; `compileOnly(project(":rewrite-toml"))`; `testImplementation(project(":rewrite-test"))`; `testImplementation(project(":rewrite-gradle"))`; `testImplementation(project(":rewrite-core"))`; `testImplementation(project(":rewrite-maven"))`

## rewrite-gradle-tooling-model:plugin

**Gradle Tooling API model registration.** ToolingApiOpenRewriteModelPlugin implements Gradle Plugin<Project> and registers the model-building integration. Its implementation depends on the model project and rewrite-gradle. It is always included by settings, even with a selective IDE import, because the colon-qualified project name is not selected through IDE.properties. Its model serializer explicitly includes Maven repository username/password properties, so extracted model JSON must be handled as potentially credential-bearing data and must not be pasted into public reports.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle-tooling-model/plugin/src/main/java/org/openrewrite/gradle/toolingapi/ToolingApiOpenRewriteModelPlugin.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle-tooling-model/plugin/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle-tooling-model/plugin)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-gradle-tooling-modelplugin); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-gradle-tooling-model/model/src/test](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle-tooling-model/model/src/test); suggested focused entry: `./gradlew :rewrite-gradle-tooling-model:plugin:test`
- Internal dependency declarations: `implementation(project(":rewrite-gradle-tooling-model:model"))`; `implementation(project(":rewrite-gradle"))`

## rewrite-groovy

**Groovy parser and shared-J language layer.** GroovyParser constructs source-faithful trees using the Groovy compiler front end and shared Java tree types plus G-specific nodes and markers. It suppresses global AST transformations because transformed compiler nodes can lose the original source-position relationship. Formatting, template, style and service packages support recipes. check also includes a separately configured groovy2Test suite.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-groovy/src/main/java/org/openrewrite/groovy/GroovyParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-groovy/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-groovy)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-groovy); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-groovy/src/test/java/org/openrewrite/groovy/GroovyParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-groovy/src/test/java/org/openrewrite/groovy/GroovyParserTest.java); suggested focused entry: `./gradlew :rewrite-groovy:test`
- Internal dependency declarations: `implementation(project())`; `implementation(project(":rewrite-test"))`; `api(project(":rewrite-java"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`; `testImplementation(project(":rewrite-java-test"))`; `testRuntimeOnly(project(":rewrite-java-21"))`

## rewrite-hcl

**HCL parser, templates and formatting.** HclParser combines generated ANTLR syntax with Hcl trees and visitors. Search, style and template packages let recipes select and replace configuration content while retaining its formatting representation. generateAntlrSources consumes the checked-in grammars and writes generated Java; it is distinct from editing the handwritten parser visitor or printer.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-hcl/src/main/java/org/openrewrite/hcl/HclParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-hcl/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-hcl)
- Packages/source sets: [owned package guide](packages-formats.md#rewrite-hcl); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-hcl/src/test/java/org/openrewrite/hcl/HclParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-hcl/src/test/java/org/openrewrite/hcl/HclParserTest.java); suggested focused entry: `./gradlew :rewrite-hcl:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`

## rewrite-java

**Shared Java semantic model and transformations.** Owns JavaParser's common interface, J trees, JavaVisitor/JavaIsoVisitor, matching, templates, formatting, searches, traits and recipes. Concrete javac-backed parsers live in the version-specific projects. Other JVM and RPC languages reuse J shapes where semantics match; their own node types and printers preserve differences. The build uses a Java 21 toolchain, shadowed dependencies and ANTLR-generated signature grammars.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/JavaParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-java); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-java/src/test](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/test); suggested focused entry: `./gradlew :rewrite-java:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `api(project(":rewrite-yaml"))`; `compileOnly(project(":rewrite-test"))`; `runtimeOnly(project(":rewrite-properties"))`; `runtimeOnly(project(":rewrite-xml"))`; `testImplementation(project(":rewrite-test"))`; `testImplementation(project(":rewrite-java-test"))`; `testRuntimeOnly(project(":rewrite-java-21"))`

## rewrite-java-11

**Java 11 isolated parser implementation.** Provides Java11Parser and isolated javac-facing visitor, type mapping and compiler support. It depends on the shared Java/core model and Lombok integration; its compiler target is Java 11. compatibilityTest imports TCK/test utilities and is attached to check. Isolated implementation types are parser plumbing rather than a replacement for the JavaParser API.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-11/src/main/java/org/openrewrite/java/Java11Parser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-11/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-11)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-java-11); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-java-tck/src/main/java/org/openrewrite/java/tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-tck/src/main/java/org/openrewrite/java/tree); suggested focused entry: `./gradlew :rewrite-java-11:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `api(project(":rewrite-java"))`; `implementation(project(":rewrite-java-lombok"))`; `testImplementation(project(":rewrite-test"))`; `implementation(project())`; `implementation(project(":rewrite-test"))`; `implementation(project(":rewrite-java-tck"))`; `implementation(project(":rewrite-java-test"))`

## rewrite-java-17

**Java 17 isolated parser implementation.** Provides Java17Parser with compiler-specific machinery in org.openrewrite.java.isolated, while reusing shared J trees and recipe APIs. The module requests Java 17 and attaches compatibilityTest to check. The settings omit this project in IDE mode but include it in an ordinary command-line build.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-17/src/main/java/org/openrewrite/java/Java17Parser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-17/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-17)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-java-17); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-java-tck/src/main/java/org/openrewrite/java/tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-tck/src/main/java/org/openrewrite/java/tree); suggested focused entry: `./gradlew :rewrite-java-17:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `api(project(":rewrite-java"))`; `implementation(project(":rewrite-java-lombok"))`; `testImplementation(project(":rewrite-test"))`; `implementation(project())`; `implementation(project(":rewrite-test"))`; `implementation(project(":rewrite-java-tck"))`; `implementation(project(":rewrite-java-test"))`

## rewrite-java-21

**Java 21 isolated parser implementation.** Provides Java21Parser and isolated compiler-to-LST mapping. It requests a Java 21 toolchain and is the test-runtime parser dependency used by many sibling modules. Its compatibilityTest suite imports the Java TCK and java-test fixtures; running only test does not execute that separate suite.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-21/src/main/java/org/openrewrite/java/Java21Parser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-21/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-21)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-java-21); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-java-21/src/test/java/org/openrewrite/java/Java21ParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-21/src/test/java/org/openrewrite/java/Java21ParserTest.java); suggested focused entry: `./gradlew :rewrite-java-21:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `api(project(":rewrite-java"))`; `implementation(project(":rewrite-java-lombok"))`; `testImplementation(project(":rewrite-test"))`; `implementation(project())`; `implementation(project(":rewrite-test"))`; `implementation(project(":rewrite-java-tck"))`; `implementation(project(":rewrite-java-test"))`

## rewrite-java-25

**Java 25 isolated parser implementation.** Provides Java25Parser and isolated compiler-to-LST mapping using a Java 25 toolchain. It reuses core, the Java model and Lombok adapters. The compatibilityTest suite is part of check and has a different coverage purpose from this module's own tests; both remain unexecuted in this documentation review.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-25/src/main/java/org/openrewrite/java/Java25Parser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-25/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-25)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-java-25); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-java-tck/src/main/java/org/openrewrite/java/tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-tck/src/main/java/org/openrewrite/java/tree); suggested focused entry: `./gradlew :rewrite-java-25:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `api(project(":rewrite-java"))`; `implementation(project(":rewrite-java-lombok"))`; `testImplementation(project(":rewrite-test"))`; `implementation(project())`; `implementation(project(":rewrite-test"))`; `implementation(project(":rewrite-java-tck"))`; `implementation(project(":rewrite-java-test"))`

## rewrite-java-8

**Java 8 parser implementation.** Provides the Java 8 parser and javac-facing mapping for the common JavaParser contract. Compilation and test launchers request Java 8. It consumes the Java model and Lombok integration and imports the Java TCK into a separate compatibilityTest suite. The directory remains in the source inventory even when IDE mode omits it.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-8/src/main/java/org/openrewrite/java/Java8Parser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-8/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-8)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-java-8); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-java-8/src/test](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-8/src/test); suggested focused entry: `./gradlew :rewrite-java-8:test`
- Internal dependency declarations: `implementation(project(":rewrite-java"))`; `implementation(project(":rewrite-java-lombok"))`; `testImplementation(project(":rewrite-test"))`; `implementation(project())`; `implementation(project(":rewrite-test"))`; `implementation(project(":rewrite-java-tck"))`; `implementation(project(":rewrite-java-test"))`

## rewrite-java-lombok

**Lombok parser integration.** Supplies javac annotation handlers and support loaded by version-specific parsers. The unpackAndRenameLombok build task prepares Lombok classes for this integration; the build requests Java 8. These handlers support parsing code that uses Lombok and should not be confused with application-level recipe transformations.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-lombok/src/main/java/org/openrewrite/java/lombok/LockedNoOpHandler.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-lombok/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-lombok)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-java-lombok); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-java-lombok/build.gradle.kts](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-lombok/build.gradle.kts); suggested focused entry: `./gradlew :rewrite-java-lombok:test`
- Internal dependency declarations: `implementation(project(":rewrite-core"))`

## rewrite-java-tck

**Shared Java parser compatibility corpus.** Packages parser/AST tests under src/main/java so version-specific parser modules can reuse them in compatibilityTest. It depends on rewrite-java, rewrite-java-test and rewrite-test. Main source placement here does not make the PackageTest/ParserTest classes production transformation APIs. Existing README explains its cross-parser testing role, but its Java 17 runtime diagram is historical: the current manifest adds Java 21 runtimeOnly during IDEA import/sync.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-tck/README.md); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-tck/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-tck)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-java-tck); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-java-tck/src/main/java/org/openrewrite/java/tree/ParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-tck/src/main/java/org/openrewrite/java/tree/ParserTest.java); suggested focused entry: `./gradlew :rewrite-java-tck:test`
- Internal dependency declarations: `implementation(project(":rewrite-java"))`; `implementation(project(":rewrite-java-test"))`; `implementation(project(":rewrite-test"))`; `runtimeOnly(project(":rewrite-java-21"))`

## rewrite-java-test

**Java type fixtures and shared test support.** Contains JavaTypeGoat and related type-system fixtures in main plus extensive Java parser/recipe tests. It depends on the Java model and reusable test harness; its build selects Java 21 and supplies Java 21 parsing at test runtime. Use it to reproduce type-attribution shapes, not as the concrete parser implementation.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-test/src/main/java/org/openrewrite/java/JavaTypeGoat.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-test/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-test)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-java-test); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-java-test/src/test/java/org/openrewrite/java/JavaParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-test/src/test/java/org/openrewrite/java/JavaParserTest.java); suggested focused entry: `./gradlew :rewrite-java-test:test`
- Internal dependency declarations: `implementation(project(":rewrite-java"))`; `implementation(project(":rewrite-test"))`; `testRuntimeOnly(project(":rewrite-java-21"))`; `testRuntimeOnly(project(":rewrite-yaml"))`; `testImplementation(project(":rewrite-properties"))`; `testImplementation(project(":rewrite-xml"))`

## rewrite-javascript

**Java host and TypeScript/JavaScript runtime.** The Java library provides JS/J trees, JavaScriptParser, RPC/process management, dependency/lock-file operations and marketplace integration. Nested @openrewrite/rewrite contains TypeScript implementations, the rewrite-rpc executable and exported JavaScript, Java, JSON, YAML, text and test surfaces. RPC type names and sender/receiver ordering must match the Java side. Native build scripts delete generated outputs, chmod the server and run npm link; they are not read-only documentation commands.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/src/main/java/org/openrewrite/javascript/JavaScriptParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript)
- Packages/source sets: [owned package guide](packages-polyglot.md#rewrite-javascript); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-javascript/src/integTest/java/org/openrewrite/javascript/JavaScriptParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/src/integTest/java/org/openrewrite/javascript/JavaScriptParserTest.java); suggested focused entry: `./gradlew :rewrite-javascript:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `api(project(":rewrite-java"))`; `api(project(":rewrite-json"))`; `implementation(project(":rewrite-yaml"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`; `testRuntimeOnly(project(":rewrite-java-21"))`; `testRuntimeOnly(project(":rewrite-maven"))`; `implementation(project())`; `implementation(project(":rewrite-java-21"))`; `implementation(project(":rewrite-test"))`; `implementation(project(":rewrite-json"))`; `implementation(project(":rewrite-java-tck"))`

## rewrite-json

**JSON parser, tree model and recipes.** JsonParser and JsonVisitor work with Json documents, keys, values and spacing/comment structures. JSON-path grammar supports selection; formatting, search, comment services and RPC codecs sit above the tree layer. JsonCommentService handles comments stored in node prefixes. Read parser acceptance and source tests for actual supported syntax rather than assuming every JSON-like dialect is accepted.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-json/src/main/java/org/openrewrite/json/JsonParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-json/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-json)
- Packages/source sets: [owned package guide](packages-formats.md#rewrite-json); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-json/src/test/java/org/openrewrite/json/JsonParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-json/src/test/java/org/openrewrite/json/JsonParserTest.java); suggested focused entry: `./gradlew :rewrite-json:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`

## rewrite-kotlin

**Kotlin parser and recipe-authoring DSL.** Contains KotlinParser/KotlinVisitor/K trees, compiler-backed type mapping, formatting and templates, plus a Kotlin recipe DSL and K2 compiler-plugin support. buildSrc generates 13 cross-language visitor scopes from compiled visitor classes; KotlinScope is handwritten. Cross-language scope dependencies are compileOnly, so recipe consumers must add the language modules they use. Gradle scope generation is intentionally absent to avoid a project cycle. The build pins Kotlin 2.4.20, uses JDK 21 and targets JVM 1.8 for main Kotlin output.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/kotlin/org/openrewrite/RecipeDsl.kt); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-kotlin); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-kotlin/src/test/kotlin/org/openrewrite/RecipeDslSurfaceTest.kt](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/test/kotlin/org/openrewrite/RecipeDslSurfaceTest.kt); suggested focused entry: `./gradlew :rewrite-kotlin:test`
- Internal dependency declarations: `compileOnly(project(":rewrite-core"))`; `compileOnly(project(":rewrite-test"))`; `implementation(project(":rewrite-java"))`; `compileOnly(project(":rewrite-yaml"))`; `compileOnly(project(":rewrite-xml"))`; `compileOnly(project(":rewrite-maven"))`; `compileOnly(project(":rewrite-json"))`; `compileOnly(project(":rewrite-properties"))`; `compileOnly(project(":rewrite-toml"))`; `compileOnly(project(":rewrite-hcl"))`; `compileOnly(project(":rewrite-python"))`; `compileOnly(project(":rewrite-csharp"))`; `compileOnly(project(":rewrite-scala"))`; `compileOnly(project(":rewrite-javascript"))`; `compileOnly(project(":rewrite-groovy"))`; `testImplementation(project(":rewrite-test"))`; `testRuntimeOnly(project(":rewrite-java-21"))`

## rewrite-maven

**Maven POM resolution and recipes.** MavenParser parses POMs as XML plus resolution markers, active profiles and dependency data. Cache, downloader, repository/settings and graph packages support resolving dependency relationships; recipes modify that model and XML. Resolution can involve network and local caches, so parser work is not necessarily side-effect-free. JavaRewriteRpc in this module is also the standalone Java recipe RPC server used by native-language integrations.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/MavenParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-maven); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-maven/src/test/java/org/openrewrite/maven/MavenParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/test/java/org/openrewrite/maven/MavenParserTest.java); suggested focused entry: `./gradlew :rewrite-maven:test`
- Internal dependency declarations: `api(project(":rewrite-xml"))`; `implementation(project(":rewrite-core"))`; `compileOnly(project(":rewrite-test"))`; `implementation(project(":rewrite-java"))`; `compileOnly(project(":rewrite-yaml"))`; `implementation(project(":rewrite-properties"))`; `testImplementation(project(":rewrite-test"))`; `testRuntimeOnly(project(":rewrite-java-21"))`

## rewrite-properties

**Java properties files as structured source.** PropertiesParser produces Properties.File entries and comment nodes with continuation/format information. Recipes find/change keys and values; PropertiesReference exposes reference traits. PropertiesCommentService treats comments as sibling nodes, unlike the prefix-based comments in JSON/YAML. It depends on core without a javac parser requirement.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-properties/src/main/java/org/openrewrite/properties/PropertiesParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-properties/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-properties)
- Packages/source sets: [owned package guide](packages-formats.md#rewrite-properties); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-properties/src/test/java/org/openrewrite/properties/PropertiesParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-properties/src/test/java/org/openrewrite/properties/PropertiesParserTest.java); suggested focused entry: `./gradlew :rewrite-properties:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`

## rewrite-protobuf

**Protocol Buffer schema parser.** ProtoParser, ProtoVisitor and Proto trees represent schema text rather than generated message implementations. The generated grammar is named Protobuf2; ImplicitProto2Syntax records an omitted syntax declaration. Do not infer universal proto3/edition support from the module name. Generated grammar code, handwritten parse mapping, markers and printer have separate responsibilities.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-protobuf/src/main/java/org/openrewrite/protobuf/ProtoParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-protobuf/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-protobuf)
- Packages/source sets: [owned package guide](packages-formats.md#rewrite-protobuf); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-protobuf/src/test/java/org/openrewrite/protobuf/ProtoParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-protobuf/src/test/java/org/openrewrite/protobuf/ProtoParserTest.java); suggested focused entry: `./gradlew :rewrite-protobuf:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`

## rewrite-python

**Java host and Python-native language/runtime package.** PythonParser delegates language parsing to the native rewrite package. Java code additionally handles pyproject/Pipfile/requirements inputs, dependency metadata, PEP 440/508, index metadata and native lock regeneration for several formats. The Python distribution supplies tree/visitor/recipe machinery, parser/printer, templates and RPC. Its runtime requirement is Python >=3.12; parsing older Python syntax does not lower that interpreter requirement. Gradle creates/uses a local virtual environment and wires native pytest and Python-2 syntax compatibility separately.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/main/java/org/openrewrite/python/PythonParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python)
- Packages/source sets: [owned package guide](packages-polyglot.md#rewrite-python); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-python/src/test/java/org/openrewrite/python/PythonParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/src/test/java/org/openrewrite/python/PythonParserTest.java); suggested focused entry: `./gradlew :rewrite-python:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `api(project(":rewrite-java"))`; `api(project(":rewrite-toml"))`; `implementation(project(":rewrite-json"))`; `implementation(project(":rewrite-maven"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`; `testRuntimeOnly(project(":rewrite-java-21"))`; `implementation(project())`; `implementation(project(":rewrite-java-21"))`; `implementation(project(":rewrite-test"))`

## rewrite-ruby

**Prism-backed Ruby parser.** RubyParser uses the Prism WASM/API packages directly rather than starting JRuby. Prism byte offsets are translated through PrismSource, then RubyParserVisitor builds J/Rb nodes with printing markers. A shared Prism instance is synchronized because parsing is not reentrant. Ruby requires Java 21; syntax that cannot be mapped is reported rather than intentionally downgraded to J.Unknown. Corpus tests are opt-in and distinct from ordinary unit tests.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-ruby/src/main/java/org/openrewrite/ruby/RubyParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-ruby/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-ruby)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-ruby); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-ruby/src/test/java/org/openrewrite/ruby/RubyParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-ruby/src/test/java/org/openrewrite/ruby/RubyParserTest.java); suggested focused entry: `./gradlew :rewrite-ruby:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `api(project(":rewrite-java"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`; `testImplementation(project(":rewrite-java-test"))`; `testRuntimeOnly(project(":rewrite-java-21"))`

## rewrite-scala

**Scala compiler bridge and Scala-specific trees.** ScalaParser and the Java-side visitors/printer work with a Scala 3 compiler bridge, type-signature builder and S nodes layered on J. Java and Scala files are jointly compiled by compileScala; compileJava excludes Java sources already handled there. The build filters Scala prereleases and launches Scala compilation with Java 17. Existing README and CLAUDE.md explain syntax mapping; their historical corpus claims are not fresh test results.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-scala/src/main/java/org/openrewrite/scala/ScalaParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-scala/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-scala)
- Packages/source sets: [owned package guide](packages-jvm.md#rewrite-scala); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-scala/src/test/java/org/openrewrite/scala/ScalaParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-scala/src/test/java/org/openrewrite/scala/ScalaParserTest.java); suggested focused entry: `./gradlew :rewrite-scala:test`
- Internal dependency declarations: `api(project(":rewrite-java"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`; `testImplementation(project(":rewrite-java-test"))`; `testRuntimeOnly(project(":rewrite-java-21"))`

## rewrite-test

**Reusable recipe-test harness.** Exports RewriteTest, RecipeSpec and SourceSpec support for parsing before-text, applying recipes and comparing expected after-text. It is a library consumed by other test suites, not an application. Its JUnit and AssertJ dependencies belong to the test-authoring surface. Runner overrides can select a different execution backend; a test filename or a configured runner is not a receipt that the backend ran.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-test/src/main/java/org/openrewrite/test/RewriteTest.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-test/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-test)
- Packages/source sets: [owned package guide](packages-core.md#rewrite-test); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-test/src/test](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-test/src/test); suggested focused entry: `./gradlew :rewrite-test:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `testImplementation(project(":rewrite-java"))`; `testImplementation(project(":rewrite-groovy"))`; `testRuntimeOnly(project(":rewrite-java-21"))`

## rewrite-toml

**TOML trees and configuration edits.** TomlParser uses ANTLR and a handwritten mapping to Toml keys, values, tables and spacing. Recipes change/delete keys, table values and rows or create files. ArrayTable and InlineTable preserve syntax distinctions through markers; the printer reconstructs source from these shapes. It is reused by Gradle version catalogs and Python dependency-file support.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-toml/src/main/java/org/openrewrite/toml/TomlParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-toml/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-toml)
- Packages/source sets: [owned package guide](packages-formats.md#rewrite-toml); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-toml/src/test/java/org/openrewrite/toml/TomlParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-toml/src/test/java/org/openrewrite/toml/TomlParserTest.java); suggested focused entry: `./gradlew :rewrite-toml:test`
- Internal dependency declarations: `implementation(project(":rewrite-core"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`

## rewrite-xml

**XML trees, XPath and formatting services.** XmlParser maps XML grammar output into Xml source trees used directly and by Maven/C# project support. Search and recipe helpers operate on tags, attributes and namespaces. Comments are explicit Xml.Comment nodes. HTML-like void-element tolerance is path-sensitive in the parser and recorded with HtmlVoidElement. The build can generate both Java XML grammar code and C# grammar code in the sibling C# SDK.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/src/main/java/org/openrewrite/xml/XmlParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml)
- Packages/source sets: [owned package guide](packages-formats.md#rewrite-xml); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-xml/src/test/java/org/openrewrite/xml/XmlParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-xml/src/test/java/org/openrewrite/xml/XmlParserTest.java); suggested focused entry: `./gradlew :rewrite-xml:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`

## rewrite-yaml

**YAML trees, selection and configuration recipes.** YamlParser uses SnakeYAML events and source tracking to build documents, mappings, sequences, scalars and aliases. JsonPath grammar is for selection rather than the YAML parser itself. Formatting and comment services operate on prefix strings; YamlKey permits scalar or alias blocks. Recipes modify keys/values, merge documents and clean up unused syntax.

- Sources: [entry/contract](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml/src/main/java/org/openrewrite/yaml/YamlParser.java); [build manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml/build.gradle.kts); [project tree](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml)
- Packages/source sets: [owned package guide](packages-formats.md#rewrite-yaml); test/fixture areas are separately listed in [coverage](coverage.md)
- Verification reference: [rewrite-yaml/src/test/java/org/openrewrite/yaml/YamlParserTest.java](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-yaml/src/test/java/org/openrewrite/yaml/YamlParserTest.java); suggested focused entry: `./gradlew :rewrite-yaml:test`
- Internal dependency declarations: `api(project(":rewrite-core"))`; `compileOnly(project(":rewrite-test"))`; `testImplementation(project(":rewrite-test"))`

## Root, build tooling and structural containers

- **Root rewrite build**: [root manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/build.gradle.kts) applies build conventions, assigns group `org.openrewrite`, rejects JUnit snapshots for integration-test configurations and post-processes generated ANTLR headers. `cleanReports` deletes module report directories. These are build effects, not recipe APIs. Root settings and the wrapper are the setup entry points.
- **buildSrc**: [manifest](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/buildSrc/build.gradle.kts) uses Kotlin DSL and ASM. [GenerateLanguageScopesTask](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/buildSrc/src/main/kotlin/org/openrewrite/codegen/dsl/GenerateLanguageScopesTask.kt) reads visitor class files without reflective classloading, walks visit methods up to TreeVisitor, removes stale Scope.kt outputs and writes generated Kotlin. It feeds rewrite-kotlin's `generateLanguageScopes`; no generated output was regenerated here.
- **rewrite-gradle-tooling-model container**: the two explicit child projects share a structural parent path. There is no parent build.gradle.kts in this tree; do not invent an independently published parent library.
- **Generated/analysis/configuration areas**: ANTLR outputs, resources, test fixture corpora, .moderne context, .m3 analysis scripts, .github workflows, Gradle wrapper/conventions and IDE files are accounted for in [coverage](coverage.md). They have different owners and must not be counted as unreviewed application packages merely because they are directories.

## Nested native build and distribution units

### C# solution and three projects

[OpenRewrite.sln](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite.sln) contains three net10.0 projects:

- [OpenRewrite SDK](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite/OpenRewrite.csproj): produces the `OpenRewrite.CSharp` NuGet library. Core, shared-J, CSharp and Xml namespaces implement the runtime; Roslyn provides compiler/workspace services, NuGet/MSBuild support dependency/project loading, and StreamJsonRpc connects peers. Test helpers are included in the SDK, but the test runner is a sibling project.
- [OpenRewrite.Tool](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite.Tool/OpenRewrite.Tool.csproj): executable/tool package `OpenRewrite.CSharp.Tool`, command `rewrite-csharp`. [Program.cs](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite.Tool/Program.cs) is the RPC-server entry; its project reference points to the SDK (overridable with `OpenRewriteSdkSourcePath`).
- [OpenRewrite.Tests](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-csharp/csharp/OpenRewrite.Tests/OpenRewrite.Tests.csproj): non-packable xUnit test project referencing the SDK. Its LibGit2Sharp dependency is test-only; do not infer that the production tool ships that library.

From the repository root use `./gradlew :rewrite-csharp:csharpBuild` and `./gradlew :rewrite-csharp:csharpTest`; Java bridge tests use `:rewrite-csharp:integTest`. For direct .NET test execution, target `OpenRewrite.Tests/OpenRewrite.Tests.csproj` from `rewrite-csharp/csharp`. The Gradle manifest explicitly targets that project. The older module guide's `OpenRewrite/Tests` layout and description of `:test` as the bridge suite are stale at this source revision; the checked-in tree and build wiring take precedence.

### Go module

[go.mod](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/go.mod) declares `github.com/openrewrite/rewrite/rewrite-go` and Go 1.25.0. [cmd](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/cmd) has the RPC executable, an exploration tool and an export-data generator; [pkg](https://github.com/hsoliwal/rewrite/tree/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-go/pkg) is the importable implementation. The Gradle module can stage source plus vendored dependencies under META-INF and build `build/rewrite-go-rpc`. `./gradlew :rewrite-go:goBuild` and `:rewrite-go:goTest` are the explicit native entries. The latter uses gotestsum and a generated Java RPC classpath; it is broader than compiling cmd/rpc alone. No vendor directory is silently treated as first-party source.

### npm distribution and auxiliary recipe dependencies

[@openrewrite/rewrite package.json](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/package.json) declares the root export plus `./text`, `./json`, `./java`, `./javascript`, `./rpc`, `./test`, `./test/java-rpc` and `./yaml`. Those are package entry points; internal source directories are not separate npm distributions. The `rewrite-rpc` bin points to dist/rpc/server.js. TypeScript build output goes to dist, and `npm test` chains type-checking, build and Vitest. The build's postbuild step also runs npm link, so it changes local package-link state. Source settings and package scripts are authoritative; no supported Node-version matrix was executed.

The separate [.rewrite/package.json](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-javascript/rewrite/.rewrite/package.json) has the name `please-work` and a dependency on `@openrewrite/recipes-npm`, but no script or entry-point declaration. It is accounted as auxiliary recipe dependency configuration with **unresolved standalone-project intent**, not claimed as a second shipping runtime. Lock-file fixture package.json files are cataloged separately.

### Python distribution

[pyproject.toml](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-python/rewrite/pyproject.toml) declares distribution `openrewrite`, setuptools discovery of `rewrite`/`rewrite.*` under src, Python >=3.12, the `openrewrite.recipes` activation entry and the `rewrite-python-types` script. It depends on more_itertools, ty-types and parso; dev extras include pytest/pytest-timeout. Outputs are a Python distribution and the installed package, distinct from the Java JAR.

From `rewrite-python/rewrite`, existing guidance uses `uv pip install -e ".[dev]"` and `pytest tests/ -v --timeout=60`. Java-backed RPC tests need the classpath produced by `./gradlew :rewrite-python:generateTestClasspath`. Gradle's `pythonInstall` prepares the local virtual environment and `pytestTest` runs native tests. The pyproject tox env_list still includes py311 despite requires-python >=3.12; this is recorded as configuration/documentation drift, not a supported runtime claim.

### M3 Maven sidecars

- [m3-recipe-first-bootstrap](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/.m3/openrewrite-recipes/pom.xml): a separate Java 21 Maven project pinned to OpenRewrite 8.17.1, containing the two public bootstrap Recipe types. It is not compiled against the current Gradle modules merely because it lives in this repository. `mvn -f .m3/openrewrite-recipes/pom.xml clean install` is the existing build instruction and writes the local Maven repository.
- [m3-repository-analysis](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/.m3/analysis-pom.xml): Maven analysis entry with OpenRewrite Maven plugin 5.23.1 and inventory/task profiles. It points sourceRoot at the repository parent of .m3 and excludes .git/.m3/target/build/out/node_modules paths. It consumes the bootstrap artifact; it is not the native Gradle reactor. The task profile requires `m3.llm.taskCrateFile` and `m3.llm.taskCrateRoot`.

[M3BootstrapTaskRecipe](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/.m3/openrewrite-recipes/src/main/java/com/synexia/m3/bootstrap/M3BootstrapTaskRecipe.java) checks a non-symlink regular file, nonempty size up to 4 MiB, and a 64-character SHA-256 value. It throws on missing/invalid data and then returns M3BootstrapInventoryRecipe. The inventory visitor emits source/type/method/shape/arity rows and does not edit the visited Java nodes. The separate atomize/patternize scripts refer to an external recipe artifact and content-bound inventory; their existence is not evidence that those recipes or an inventory have run here.
