# JVM languages and build systems: package responsibilities

[Architecture index](README.md) · [Module guide](modules.md) · [Coverage ledger](coverage.md)

Source baseline: `0c82f71bc8bb215a8194db59a3784551d6a60e99`. Entries describe source-directory groups and observed declarations. Java/Kotlin/Scala namespaces can span languages/artifacts; Go uses import paths, C# namespaces, Python import packages and TypeScript file modules. One representative declaration was read per group, so these are **partial responsibility summaries**, not an exhaustive namespace/member census. Listed surface names are examples present in the tree, not a claim that every type is public. Existing API documentation remains authoritative except where a specific drift is recorded.

Every entry retains its exact source directory and representative evidence in [coverage](coverage.md). The linked package-info files often contain only nullness declarations. All tests are source references, **not executed receipts**. Public-member parameters/returns/errors, side effects, lifecycle, dependency edges and individual Python/TypeScript file-module semantics still need full reconciliation; this common gap applies to every entry below.

## rewrite-android

### org.openrewrite.android

Recipes update Android Gradle plugin and SDK declarations across Gradle scripts, properties and version catalogs. Android markers describe project variants and SDK metadata; traits recognize Android-relevant Java/Kotlin syntax. There is no AndroidVisitor: package-level documentation explicitly composes traits with visitors/scanning recipes across source languages and XML resources.

production; 4 source files. Surface: `UpgradeAndroidGradlePluginVersion`, `UpgradeCompileSdkVersion`, `UpgradeMinSdkVersion`, `UpgradeTargetSdkVersion`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-android/src/main/java/org/openrewrite/android/UpgradeCompileSdkVersion.java); [test-source reference](../../rewrite-android/src/test/java/org/openrewrite/android/UpgradeAndroidGradlePluginVersionTest.java).

### org.openrewrite.android.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 8 source files. Surface: `AbstractUpgradeSdkVersion`, `SdkVersionValueSource`, `SdkVersionValueSourceResolver`, `UpgradeSdkExtraPropertyVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-android/src/main/java/org/openrewrite/android/internal/UpgradeSdkGradlePropertiesVisitor.java); [package annotations](../../rewrite-android/src/main/java/org/openrewrite/android/internal/package-info.java); test relationship unresolved.

### org.openrewrite.android.marker

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 8 source files. Surface: `AndroidBuildFeatures`, `AndroidBuildType`, `AndroidProductFlavor`, `AndroidProject`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-android/src/main/java/org/openrewrite/android/marker/AndroidSdkVersions.java); [package annotations](../../rewrite-android/src/main/java/org/openrewrite/android/marker/package-info.java); [test-source reference](../../rewrite-android/src/test/java/org/openrewrite/android/marker/AndroidProjectSerializationTest.java).

### org.openrewrite.android.trait

Cursor-backed semantic views and matchers for language-specific constructs or references; use specific Matcher types where the Traits facade is deprecated.

production; 6 source files. Surface: `AndroidFindViewById`, `AndroidIntentAction`, `AndroidLifecycleMethod`, `AndroidPermissionCheck`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-android/src/main/java/org/openrewrite/android/trait/AndroidFindViewById.java); [package Javadoc](../../rewrite-android/src/main/java/org/openrewrite/android/trait/package-info.java); [test-source reference](../../rewrite-android/src/test/java/org/openrewrite/android/trait/AndroidFindViewByIdTest.java).

## rewrite-gradle-tooling-model:model

### org.openrewrite.gradle.marker

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 4 source files. Surface: `GradleProjectBuilder`, `GradleSettingsBuilder`, `ReflectiveFeaturePreviewFetcher`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle-tooling-model/model/src/main/java/org/openrewrite/gradle/marker/ReflectiveFeaturePreviewFetcher.java); [package annotations](../../rewrite-gradle-tooling-model/model/src/main/java/org/openrewrite/gradle/marker/package-info.java); [test-source reference](../../rewrite-gradle-tooling-model/model/src/test/java/org/openrewrite/gradle/marker/GradleProjectBuilderTest.java).

### org.openrewrite.gradle.toolingapi

Gradle extraction model and Tooling API bridge; model and plugin artifacts share the namespace but have distinct responsibilities.

production; 6 source files. Surface: `Assertions`, `OpenRewriteModel`, `OpenRewriteModelBuilder`, `OpenRewriteModelImpl`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle-tooling-model/model/src/main/java/org/openrewrite/gradle/toolingapi/OpenRewriteModelImpl.java); [package Javadoc](../../rewrite-gradle-tooling-model/model/src/main/java/org/openrewrite/gradle/toolingapi/package-info.java); [test-source reference](../../rewrite-gradle-tooling-model/model/src/test/java/org/openrewrite/gradle/toolingapi/AssertionsTest.java).

## rewrite-gradle-tooling-model:plugin

### org.openrewrite.gradle.toolingapi

Gradle extraction model and Tooling API bridge; model and plugin artifacts share the namespace but have distinct responsibilities.

production; 2 source files. Surface: `ToolingApiOpenRewriteModelPlugin`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle-tooling-model/plugin/src/main/java/org/openrewrite/gradle/toolingapi/ToolingApiOpenRewriteModelPlugin.java); [package annotations](../../rewrite-gradle-tooling-model/plugin/src/main/java/org/openrewrite/gradle/toolingapi/package-info.java); test relationship unresolved.

## rewrite-gradle

### unnamed source area

Unnamed-package Groovy DSL stub interfaces used by Gradle script parsing/type attribution; this directory is not a named Java package.

production; 4 source files. Surface: `Plugin`, `RepositoryHandler`, `RewriteGradleProject`, `RewriteSettings`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/groovy/Plugin.groovy); test relationship unresolved.

### org.openrewrite.gradle

GradleParser delegates script syntax to Groovy/Kotlin parsers and attaches Gradle-specific model context. Recipes operate on dependencies, plugins, wrappers, repositories and version catalogs; marker/attribute packages represent extracted build semantics. This is the recipe/model library, not the separately distributed OpenRewrite Gradle plugin. The two tooling-model projects support extraction/testing of Gradle semantics.

production; 41 source files. Surface: `AddDependency`, `AddDependencyVisitor`, `AddExplicitDependencyVersion`, `AddPlatformDependency`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/GradleParser.java); [package annotations](../../rewrite-gradle/src/main/java/org/openrewrite/gradle/package-info.java); [test-source reference](../../rewrite-gradle/src/test/java/org/openrewrite/gradle/package-info.java).

### org.openrewrite.gradle.attributes

Serializable dependency/build metadata such as project-path attributes; Gradle project paths use colon separators rather than filesystem separators.

production; 7 source files. Surface: `Bundling`, `Category`, `DocsType`, `LibraryElements`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/attributes/ProjectAttribute.java); test relationship unresolved.

### org.openrewrite.gradle.attributes.java

Java-target attributes from Gradle dependency variants, including numeric target JVM versions.

production; 2 source files. Surface: `TargetJvmEnvironment`, `TargetJvmVersion`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/attributes/java/TargetJvmVersion.java); test relationship unresolved.

### org.openrewrite.gradle.gradle8

Recipes targeting Gradle 8 API/deprecation migrations; applicable behavior is recipe-specific.

production; 2 source files. Surface: `JacocoReportDeprecations`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/gradle8/JacocoReportDeprecations.java); [package annotations](../../rewrite-gradle/src/main/java/org/openrewrite/gradle/gradle8/package-info.java); [test-source reference](../../rewrite-gradle/src/test/java/org/openrewrite/gradle/gradle8/JacocoReportDeprecationsTest.java).

### org.openrewrite.gradle.gradle9

Recipes targeting Gradle 9 removals/migrations, including PMD target-JDK cleanup.

production; 12 source files. Surface: `OneDependencyDeclarationPerStatement`, `RemoveDeprecatedPropertyVisitor`, `RemoveExitEnvironmentVar`, `RemovePmdTargetJdk`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/gradle9/RemovePmdTargetJdk.java); [test-source reference](../../rewrite-gradle/src/test/java/org/openrewrite/gradle/gradle9/MigrateIsolatedProjectsPropertiesTest.java).

### org.openrewrite.gradle.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 9 source files. Surface: `AddDependencyVisitor`, `ChangeStringLiteral`, `DefaultImportsCustomizer`, `Dependency`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/internal/DependencyStringNotationConverter.java); [package annotations](../../rewrite-gradle/src/main/java/org/openrewrite/gradle/internal/package-info.java); [test-source reference](../../rewrite-gradle/src/test/java/org/openrewrite/gradle/internal/GradleWrapperTest.java).

### org.openrewrite.gradle.marker

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 9 source files. Surface: `FeaturePreview`, `GradleBuildscript`, `GradleDependencyConfiguration`, `GradleDependencyConstraint`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/marker/FeaturePreview.java); [package annotations](../../rewrite-gradle/src/main/java/org/openrewrite/gradle/marker/package-info.java); [test-source reference](../../rewrite-gradle/src/test/java/org/openrewrite/gradle/marker/GradleProjectTest.java).

### org.openrewrite.gradle.plugins

Gradle settings/build plugin recipes; removal and upgrade operations manipulate plugin declarations rather than executing plugins.

production; 17 source files. Surface: `AddBuildPlugin`, `AddDevelocityGradlePlugin`, `AddPluginVisitor`, `AddSettingsPlugin`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/plugins/RemoveSettingsPlugin.java); [package annotations](../../rewrite-gradle/src/main/java/org/openrewrite/gradle/plugins/package-info.java); [test-source reference](../../rewrite-gradle/src/test/java/org/openrewrite/gradle/plugins/package-info.java).

### org.openrewrite.gradle.search

Search/precondition recipes that select syntax or dependency findings; result markers/tables are distinct from rewriting matched source.

production; 15 source files. Surface: `DependencyInsight`, `DoesNotIncludeDependency`, `EffectiveGradlePluginRepositories`, `EffectiveGradleRepositories`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/search/FindDependencyHandler.java); [package annotations](../../rewrite-gradle/src/main/java/org/openrewrite/gradle/search/package-info.java); [test-source reference](../../rewrite-gradle/src/test/java/org/openrewrite/gradle/search/package-info.java).

### org.openrewrite.gradle.security

Recipes for repository/suppression-file security configuration. The recipe name is not a general security-audit guarantee.

production; 2 source files. Surface: `UseHttpsForRepositories`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/security/UseHttpsForRepositories.java); [package annotations](../../rewrite-gradle/src/main/java/org/openrewrite/gradle/security/package-info.java); [test-source reference](../../rewrite-gradle/src/test/java/org/openrewrite/gradle/security/UseHttpsForRepositoriesTest.java).

### org.openrewrite.gradle.style

Language style definitions and/or autodetection used by formatting; style values are separate from the visitor that applies them.

production; 2 source files. Surface: `Autodetect`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/style/Autodetect.java); [package annotations](../../rewrite-gradle/src/main/java/org/openrewrite/gradle/style/package-info.java); [test-source reference](../../rewrite-gradle/src/test/java/org/openrewrite/gradle/style/AutodetectTest.java).

### org.openrewrite.gradle.table

Data-table schemas emitted by the module's recipes and analysis; actual row retention/output depends on the configured DataTableStore.

production; 3 source files. Surface: `GradleWrappersInUse`, `JVMTestSuitesDefined`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/table/JVMTestSuitesDefined.java); [package annotations](../../rewrite-gradle/src/main/java/org/openrewrite/gradle/table/package-info.java); test relationship unresolved.

### org.openrewrite.gradle.trait

Cursor-backed semantic views and matchers for language-specific constructs or references; use specific Matcher types where the Traits facade is deprecated.

production; 16 source files. Surface: `ExtraProperty`, `GradleDependencies`, `GradleDependency`, `GradleMultiDependency`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/trait/Traits.java); [package annotations](../../rewrite-gradle/src/main/java/org/openrewrite/gradle/trait/package-info.java); [test-source reference](../../rewrite-gradle/src/test/java/org/openrewrite/gradle/trait/VersionCatalogTest.java).

### org.openrewrite.gradle.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 1 source files. Surface: `GradlePlugin`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/tree/GradlePlugin.java); [test-source reference](../../rewrite-gradle/src/test/java/org/openrewrite/gradle/tree/PluginsTest.java).

### org.openrewrite.gradle.util

Wrapper/distribution utility values and resolution helpers used by Gradle recipes.

production; 2 source files. Surface: `DistributionInfos`, `GradleWrapper`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-gradle/src/main/java/org/openrewrite/gradle/util/DistributionInfos.java); test relationship unresolved.

## rewrite-groovy

### org.openrewrite.groovy

GroovyParser constructs source-faithful trees using the Groovy compiler front end and shared Java tree types plus G-specific nodes and markers. It suppresses global AST transformations because transformed compiler nodes can lose the original source-position relationship. Formatting, template, style and service packages support recipes. check also includes a separately configured groovy2Test suite.

production; 18 source files. Surface: `Assertions`, `CompiledGroovySource`, `CountLinesVisitor`, `FindBinaryOperationVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-groovy/src/main/java/org/openrewrite/groovy/GroovyParser.java); [package annotations](../../rewrite-groovy/src/main/java/org/openrewrite/groovy/package-info.java); [test-source reference](../../rewrite-groovy/src/groovy2Test/java/org/openrewrite/groovy/GroovyParserTest.java).

### org.openrewrite.groovy.format

Formatting recipes and visitors that adjust spacing, indentation or line breaks in the language model.

production; 11 source files. Surface: `AutoFormat`, `AutoFormatVisitor`, `GStringCurlyBraces`, `MergeSpacesVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-groovy/src/main/java/org/openrewrite/groovy/format/AutoFormat.java); [package annotations](../../rewrite-groovy/src/main/java/org/openrewrite/groovy/format/package-info.java); [test-source reference](../../rewrite-groovy/src/test/java/org/openrewrite/groovy/format/NoWhitespaceBeforeTest.java).

### org.openrewrite.groovy.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 4 source files. Surface: `Delimiter`, `GroovyWhitespaceValidationService`, `NegativeCachingGroovyClassLoader`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-groovy/src/main/java/org/openrewrite/groovy/internal/GroovyWhitespaceValidationService.java); [package annotations](../../rewrite-groovy/src/main/java/org/openrewrite/groovy/internal/package-info.java); test relationship unresolved.

### org.openrewrite.groovy.internal.template

Internal snippet/template parsing, substitutions and generated context support; consumed by the language's public template layer.

production; 4 source files. Surface: `GroovyBlockStatementTemplateGenerator`, `GroovySubstitutions`, `GroovyTemplateParser`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-groovy/src/main/java/org/openrewrite/groovy/internal/template/GroovyTemplateParser.java); [package annotations](../../rewrite-groovy/src/main/java/org/openrewrite/groovy/internal/template/package-info.java); test relationship unresolved.

### org.openrewrite.groovy.marker

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 19 source files. Surface: `AsStyleTypeCast`, `AssertMessageComma`, `Elvis`, `EmptyArgumentListPrecedesArgument`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-groovy/src/main/java/org/openrewrite/groovy/marker/Elvis.java); [package annotations](../../rewrite-groovy/src/main/java/org/openrewrite/groovy/marker/package-info.java); test relationship unresolved.

### org.openrewrite.groovy.service

Language-specific service adapters for comments, imports, naming, formatting or validation; inspect the concrete service's effects before calling it.

production; 3 source files. Surface: `GroovyAutoFormatService`, `GroovySourceFileStatementService`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-groovy/src/main/java/org/openrewrite/groovy/service/GroovySourceFileStatementService.java); [package annotations](../../rewrite-groovy/src/main/java/org/openrewrite/groovy/service/package-info.java); test relationship unresolved.

### org.openrewrite.groovy.style

Language style definitions and/or autodetection used by formatting; style values are separate from the visitor that applies them.

production; 3 source files. Surface: `Autodetect`, `OmitParenthesesStyle`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-groovy/src/main/java/org/openrewrite/groovy/style/OmitParenthesesStyle.java); [package annotations](../../rewrite-groovy/src/main/java/org/openrewrite/groovy/style/package-info.java); [test-source reference](../../rewrite-groovy/src/test/java/org/openrewrite/groovy/style/AutodetectTest.java).

### org.openrewrite.groovy.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 6 source files. Surface: `G`, `GContainer`, `GLeftPadded`, `GRightPadded`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-groovy/src/main/java/org/openrewrite/groovy/tree/GLeftPadded.java); [package annotations](../../rewrite-groovy/src/main/java/org/openrewrite/groovy/tree/package-info.java); [test-source reference](../../rewrite-groovy/src/test/java/org/openrewrite/groovy/tree/ThrowTest.java).

## rewrite-java-11

### org.openrewrite.java

Provides Java11Parser and isolated javac-facing visitor, type mapping and compiler support. It depends on the shared Java/core model and Lombok integration; its compiler target is Java 11. compatibilityTest imports TCK/test utilities and is attached to check. Isolated implementation types are parser plumbing rather than a replacement for the JavaParser API.

production; 1 source files. Surface: `Java11Parser`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-11/src/main/java/org/openrewrite/java/Java11Parser.java); test relationship unresolved.

### org.openrewrite.java.isolated

Version-specific javac mapping classes isolated behind the owning JavaNParser; do not mix implementations from different Java parser artifacts.

production; 7 source files. Surface: `ReloadableJava11JavadocVisitor`, `ReloadableJava11ModifierResults`, `ReloadableJava11Parser`, `ReloadableJava11ParserInputFileObject`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-11/src/main/java/org/openrewrite/java/isolated/ReloadableJava11ModifierResults.java); test relationship unresolved.

## rewrite-java-17

### org.openrewrite.java

Provides Java17Parser with compiler-specific machinery in org.openrewrite.java.isolated, while reusing shared J trees and recipe APIs. The module requests Java 17 and attaches compatibilityTest to check. The settings omit this project in IDE mode but include it in an ordinary command-line build.

production; 1 source files. Surface: `Java17Parser`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-17/src/main/java/org/openrewrite/java/Java17Parser.java); test relationship unresolved.

### org.openrewrite.java.isolated

Version-specific javac mapping classes isolated behind the owning JavaNParser; do not mix implementations from different Java parser artifacts.

production; 8 source files. Surface: `ReloadableJava17JavadocVisitor`, `ReloadableJava17ModifierResults`, `ReloadableJava17Parser`, `ReloadableJava17ParserInputFileObject`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-17/src/main/java/org/openrewrite/java/isolated/ReloadableJava17ModifierResults.java); [package annotations](../../rewrite-java-17/src/main/java/org/openrewrite/java/isolated/package-info.java); test relationship unresolved.

## rewrite-java-21

### org.openrewrite.java

Provides Java21Parser and isolated compiler-to-LST mapping. It requests a Java 21 toolchain and is the test-runtime parser dependency used by many sibling modules. Its compatibilityTest suite imports the Java TCK and java-test fixtures; running only test does not execute that separate suite.

production; 1 source files. Surface: `Java21Parser`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-21/src/main/java/org/openrewrite/java/Java21Parser.java); [test-source reference](../../rewrite-java-21/src/test/java/org/openrewrite/java/Java21ParserTest.java).

### org.openrewrite.java.isolated

Version-specific javac mapping classes isolated behind the owning JavaNParser; do not mix implementations from different Java parser artifacts.

production; 8 source files. Surface: `ReloadableJava21JavadocVisitor`, `ReloadableJava21ModifierResults`, `ReloadableJava21Parser`, `ReloadableJava21ParserInputFileObject`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-21/src/main/java/org/openrewrite/java/isolated/ReloadableJava21ModifierResults.java); [package annotations](../../rewrite-java-21/src/main/java/org/openrewrite/java/isolated/package-info.java); test relationship unresolved.

## rewrite-java-25

### org.openrewrite.java

Provides Java25Parser and isolated compiler-to-LST mapping using a Java 25 toolchain. It reuses core, the Java model and Lombok adapters. The compatibilityTest suite is part of check and has a different coverage purpose from this module's own tests; both remain unexecuted in this documentation review.

production; 1 source files. Surface: `Java25Parser`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-25/src/main/java/org/openrewrite/java/Java25Parser.java); test relationship unresolved.

### org.openrewrite.java.isolated

Version-specific javac mapping classes isolated behind the owning JavaNParser; do not mix implementations from different Java parser artifacts.

production; 8 source files. Surface: `ReloadableJava25JavadocVisitor`, `ReloadableJava25ModifierResults`, `ReloadableJava25Parser`, `ReloadableJava25ParserInputFileObject`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-25/src/main/java/org/openrewrite/java/isolated/ReloadableJava25ModifierResults.java); [package annotations](../../rewrite-java-25/src/main/java/org/openrewrite/java/isolated/package-info.java); test relationship unresolved.

## rewrite-java-8

### org.openrewrite.java

Provides the Java 8 parser and javac-facing mapping for the common JavaParser contract. Compilation and test launchers request Java 8. It consumes the Java model and Lombok integration and imports the Java TCK into a separate compatibilityTest suite. The directory remains in the source inventory even when IDE mode omits it.

production; 9 source files. Surface: `Java8ModifierResults`, `Java8Parser`, `Java8ParserInputFileObject`, `ReloadableJava8JavadocVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-8/src/main/java/org/openrewrite/java/Java8ModifierResults.java); [package annotations](../../rewrite-java-8/src/main/java/org/openrewrite/java/package-info.java); [test-source reference](../../rewrite-java-8/src/test/java/org/openrewrite/java/Java8ParserTest.java).

## rewrite-java-lombok

### org.openrewrite.java.lombok

Lombok javac annotation handlers and parser support, loaded as part of the Java-version integration.

production; 21 source files. Surface: `AllArgsConstructorHandler`, `BuilderDefaultNoOpHandler`, `BuilderHandler`, `CleanupNoOpHandler`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-lombok/src/main/java/org/openrewrite/java/lombok/LockedNoOpHandler.java); [package annotations](../../rewrite-java-lombok/src/main/java/org/openrewrite/java/lombok/package-info.java); test relationship unresolved.

## rewrite-java-tck

### org.openrewrite.java

Reusable parser compatibility tests published from main sources for consumption by version-specific compatibilityTest suites; this is test infrastructure, not an application recipe package.

test-support; 6 source files. Surface: `JavaParserTypeAttributionTest`, `JavaParserTypeMappingTest`, `MinimumJava11`, `MinimumJava17`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-tck/src/main/java/org/openrewrite/java/MinimumJava17.java); test relationship unresolved.

### org.openrewrite.java.tree

Reusable parser compatibility tests published from main sources for consumption by version-specific compatibilityTest suites; this is test infrastructure, not an application recipe package.

test-support; 58 source files. Surface: `AnnotationTest`, `ArrayAccessTest`, `ArrayTypeTest`, `AssertTest`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-tck/src/main/java/org/openrewrite/java/tree/PackageTest.java); test relationship unresolved.

## rewrite-java-test

### org.openrewrite.java

Java type-system fixture declarations used by parser/type-attribution tests; unusual bounds and nested types intentionally exercise compiler mappings.

test-support; 3 source files. Surface: `JavaTypeGoat`, `JavaTypeMappingTest`, `JavaTypeSignatureBuilderTest`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java-test/src/main/java/org/openrewrite/java/JavaTypeGoat.java); [test-source reference](../../rewrite-java-test/src/main/resources/JavaTypeGoat.java).

## rewrite-java

### org.openrewrite.java

Owns JavaParser's common interface, J trees, JavaVisitor/JavaIsoVisitor, matching, templates, formatting, searches, traits and recipes. Concrete javac-backed parsers live in the version-specific projects. Other JVM and RPC languages reuse J shapes where semantics match; their own node types and printers preserve differences. The build uses a Java 21 toolchain, shadowed dependencies and ANTLR-generated signature grammars.

production; 96 source files. Surface: `AddCommentToImport`, `AddCommentToMethod`, `AddCommentToMethodInvocations`, `AddImport`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/JavaParser.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/package-info.java); [test-source reference](../../rewrite-java/src/test/java/org/openrewrite/java/package-info.java).

### org.openrewrite.java.ai

Recipe-derived source measurements for analysis, including class-definition length; the directory name alone does not imply a model call.

production; 3 source files. Surface: `ClassDefinitionLength`, `MethodDefinitionLength`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/ai/ClassDefinitionLength.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/ai/package-info.java); test relationship unresolved.

### org.openrewrite.java.cleanup

Focused cleanup transformations for this language's syntax; each Recipe owns its own preconditions and before/after semantics.

production; 4 source files. Surface: `RenameJavaDocParamNameVisitor`, `SimplifyBooleanExpressionVisitor`, `UnnecessaryParenthesesVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/cleanup/RenameJavaDocParamNameVisitor.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/cleanup/package-info.java); test relationship unresolved.

### org.openrewrite.java.filter

Namespace/nullness declaration only at this revision; no implementation type in this source directory.

stub; 1 source files. Surface: declaration-only. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/filter/package-info.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/filter/package-info.java); test relationship unresolved.

### org.openrewrite.java.format

Formatting recipes and visitors that adjust spacing, indentation or line breaks in the language model.

production; 30 source files. Surface: `AutoFormat`, `AutoFormatVisitor`, `AutodetectGeneralFormatStyle`, `BlankLines`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/format/RemoveTrailingWhitespace.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/format/package-info.java); [test-source reference](../../rewrite-java/src/test/java/org/openrewrite/java/format/ShiftFormatTest.java).

### org.openrewrite.java.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 14 source files. Surface: `ClassIdResolver`, `DefaultJavaTypeFactory`, `DefaultJavaTypeSignatureBuilder`, `FormatFirstClassPrefix`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/internal/FormatFirstClassPrefix.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/internal/package-info.java); [test-source reference](../../rewrite-java/src/test/java/org/openrewrite/java/internal/JavaReflectionTypeSignatureBuilderTest.java).

### org.openrewrite.java.internal.grammar

Generated grammar/token/parser interfaces and automata, sometimes alongside handwritten base helpers. Parser visitors consume these structures; use the owning grammar/generation workflow and preserve handwritten helpers.

generated; 12 source files. Surface: `AnnotationSignatureLexer`, `AnnotationSignatureParser`, `AnnotationSignatureParserBaseListener`, `AnnotationSignatureParserBaseVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/internal/grammar/AnnotationSignatureParser.java); test relationship unresolved.

### org.openrewrite.java.internal.parser

Owns JavaParser's common interface, J trees, JavaVisitor/JavaIsoVisitor, matching, templates, formatting, searches, traits and recipes. Concrete javac-backed parsers live in the version-specific projects. Other JVM and RPC languages reuse J shapes where semantics match; their own node types and printers preserve differences. The build uses a Java 21 toolchain, shadowed dependencies and ANTLR-generated signature grammars.

production; 9 source files. Surface: `AnnotationDeserializer`, `JavaParserCaller`, `JavaParserClasspathLoader`, `RewriteClasspathJarClasspathLoader`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/internal/parser/JavaParserClasspathLoader.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/internal/parser/package-info.java); [test-source reference](../../rewrite-java/src/test/java/org/openrewrite/java/internal/parser/TypeTableKotlinMetadataTest.java).

### org.openrewrite.java.internal.rpc

Tree-specific RPC codecs and send/receive visitors. The inspected codec delegates to matching sender/receiver implementations; their field order/type identities must stay aligned with the native peer.

production; 10 source files. Surface: `JavaContainerRpcCodec`, `JavaLeftPaddedRpcCodec`, `JavaReceiver`, `JavaRightPaddedRpcCodec`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/internal/rpc/JavaRpcCodec.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/internal/rpc/package-info.java); [test-source reference](../../rewrite-java/src/test/java/org/openrewrite/java/internal/rpc/package-info.java).

### org.openrewrite.java.internal.template

Internal snippet/template parsing, substitutions and generated context support; consumed by the language's public template layer.

production; 12 source files. Surface: `AnnotationTemplateGenerator`, `BlockStatementTemplateGenerator`, `JavaTemplateJavaExtension`, `JavaTemplateLanguageExtension`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/internal/template/JavaTemplateParser.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/internal/template/package-info.java); [test-source reference](../../rewrite-java/src/test/java/org/openrewrite/java/internal/template/PatternVariablesTest.java).

### org.openrewrite.java.marker

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 18 source files. Surface: `CompactConstructor`, `CompactSourceFile`, `ImplicitReturn`, `JavaProject`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/marker/ImplicitReturn.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/marker/package-info.java); [test-source reference](../../rewrite-java/src/test/java/org/openrewrite/java/marker/JavaVersionTest.java).

### org.openrewrite.java.search

Search/precondition recipes that select syntax or dependency findings; result markers/tables are distinct from rewriting matched source.

production; 50 source files. Surface: `ClasspathTypeCounts`, `DeclaresMethod`, `DeclaresType`, `DoesNotUseType`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/search/IsLikelyNotTest.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/search/package-info.java); [test-source reference](../../rewrite-java/src/test/java/org/openrewrite/java/search/FindDeprecatedUsesDataTablesTest.java).

### org.openrewrite.java.service

Language-specific service adapters for comments, imports, naming, formatting or validation; inspect the concrete service's effects before calling it.

production; 9 source files. Surface: `AnnotationService`, `AutoFormatService`, `ImportService`, `JavaCommentService`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/service/Span.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/service/package-info.java); [test-source reference](../../rewrite-java/src/test/java/org/openrewrite/java/service/JavaNamingServiceTest.java).

### org.openrewrite.java.style

Language style definitions and/or autodetection used by formatting; style values are separate from the visitor that applies them.

production; 27 source files. Surface: `Autodetect`, `BlankLinesStyle`, `Checkstyle`, `CheckstyleConfigLoader`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/style/UnusedImportsStyle.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/style/package-info.java); [test-source reference](../../rewrite-java/src/test/java/org/openrewrite/java/style/StyleHelperTest.java).

### org.openrewrite.java.table

Data-table schemas emitted by the module's recipes and analysis; actual row retention/output depends on the configured DataTableStore.

production; 10 source files. Surface: `ClassHierarchy`, `ClasspathTypeCount`, `CompileErrors`, `FieldsOfTypeUses`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/table/CompileErrors.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/table/package-info.java); test relationship unresolved.

### org.openrewrite.java.trait

Cursor-backed semantic views and matchers for language-specific constructs or references; use specific Matcher types where the Traits facade is deprecated.

production; 7 source files. Surface: `Annotated`, `AttributeValue`, `Literal`, `MethodAccess`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/trait/Traits.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/trait/package-info.java); [test-source reference](../../rewrite-java/src/test/java/org/openrewrite/java/trait/MethodAccessTest.java).

### org.openrewrite.java.trait.internal

Internal helpers for trait navigation, including parent/parentheses relationships; callers normally use the public trait matchers.

production; 2 source files. Surface: `MaybeParenthesesPair`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/trait/internal/MaybeParenthesesPair.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/trait/internal/package-info.java); test relationship unresolved.

### org.openrewrite.java.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 24 source files. Surface: `Comment`, `CoordinateBuilder`, `Expression`, `Flag`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-java/src/main/java/org/openrewrite/java/tree/Statement.java); [package annotations](../../rewrite-java/src/main/java/org/openrewrite/java/tree/package-info.java); [test-source reference](../../rewrite-java/src/test/java/org/openrewrite/java/tree/JRightPaddedTest.java).

## rewrite-kotlin

### org.openrewrite.kotlin (java)

Contains KotlinParser/KotlinVisitor/K trees, compiler-backed type mapping, formatting and templates, plus a Kotlin recipe DSL and K2 compiler-plugin support. buildSrc generates 13 cross-language visitor scopes from compiled visitor classes; KotlinScope is handwritten. Cross-language scope dependencies are compileOnly, so recipe consumers must add the language modules they use. Gradle scope generation is intentionally absent to avoid a project cycle. The build pins Kotlin 2.4.20, uses JDK 21 and targets JVM 1.8 for main Kotlin output.

production; 13 source files. Surface: `AddImport`, `Assertions`, `FindKotlinSources`, `KotlinIsoVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/java/org/openrewrite/kotlin/KotlinParser.java); [package annotations](../../rewrite-kotlin/src/main/java/org/openrewrite/kotlin/package-info.java); [test-source reference](../../rewrite-kotlin/src/test/java/org/openrewrite/kotlin/FindKotlinSourcesTest.java).

### org.openrewrite.kotlin.cleanup (java)

Focused cleanup transformations for this language's syntax; each Recipe owns its own preconditions and before/after semantics.

production; 8 source files. Surface: `EqualsMethodUsage`, `ImplicitParameterInLambda`, `RemoveLambdaArgumentParentheses`, `RemoveTrailingComma`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/java/org/openrewrite/kotlin/cleanup/RemoveTrailingComma.java); [package annotations](../../rewrite-kotlin/src/main/java/org/openrewrite/kotlin/cleanup/package-info.java); [test-source reference](../../rewrite-kotlin/src/test/java/org/openrewrite/kotlin/cleanup/RemoveTrailingCommaTest.java).

### org.openrewrite.kotlin.format (java)

Formatting recipes and visitors that adjust spacing, indentation or line breaks in the language model.

production; 16 source files. Surface: `AutoFormat`, `AutoFormatVisitor`, `BlankLinesVisitor`, `ImportReorderingVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/java/org/openrewrite/kotlin/format/AutoFormat.java); [package annotations](../../rewrite-kotlin/src/main/java/org/openrewrite/kotlin/format/package-info.java); [test-source reference](../../rewrite-kotlin/src/test/java/org/openrewrite/kotlin/format/TypecastParenPadKotlinTest.java).

### org.openrewrite.kotlin.internal (java)

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 6 source files. Surface: `CompiledSource`, `KotlinParsingException`, `KotlinPrinter`, `KotlinTreeParserVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/java/org/openrewrite/kotlin/internal/KotlinParsingException.java); [package annotations](../../rewrite-kotlin/src/main/java/org/openrewrite/kotlin/internal/package-info.java); test relationship unresolved.

### org.openrewrite.kotlin.internal.template (java)

Internal snippet/template parsing, substitutions and generated context support; consumed by the language's public template layer.

production; 5 source files. Surface: `KotlinAnnotationTemplateGenerator`, `KotlinBlockStatementTemplateGenerator`, `KotlinSubstitutions`, `KotlinTemplateParser`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/java/org/openrewrite/kotlin/internal/template/KotlinTemplateParser.java); [package annotations](../../rewrite-kotlin/src/main/java/org/openrewrite/kotlin/internal/template/package-info.java); test relationship unresolved.

### org.openrewrite.kotlin.marker (java)

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 30 source files. Surface: `AnnotationCallSite`, `AnnotationConstructor`, `AnnotationUseSite`, `AnonymousFunction`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/java/org/openrewrite/kotlin/marker/KObject.java); [package annotations](../../rewrite-kotlin/src/main/java/org/openrewrite/kotlin/marker/package-info.java); test relationship unresolved.

### org.openrewrite.kotlin.recipe (java)

Runtime support used by generated recipe classes; the K2 compiler-plugin implementation lives in the internal sibling namespace.

production; 1 source files. Surface: `GeneratedRecipeSupport`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/java/org/openrewrite/kotlin/recipe/GeneratedRecipeSupport.java); [test-source reference](../../rewrite-kotlin/src/test/kotlin/org/openrewrite/kotlin/recipe/RecipePluginCompileFixture.kt).

### org.openrewrite.kotlin.search (java)

Namespace/nullness declaration only at this revision; no search implementation in this source directory.

stub; 1 source files. Surface: declaration-only. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/java/org/openrewrite/kotlin/search/package-info.java); [package annotations](../../rewrite-kotlin/src/main/java/org/openrewrite/kotlin/search/package-info.java); test relationship unresolved.

### org.openrewrite.kotlin.service (java)

Language-specific service adapters for comments, imports, naming, formatting or validation; inspect the concrete service's effects before calling it.

production; 3 source files. Surface: `KotlinAutoFormatService`, `KotlinImportService`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/java/org/openrewrite/kotlin/service/KotlinImportService.java); [package annotations](../../rewrite-kotlin/src/main/java/org/openrewrite/kotlin/service/package-info.java); test relationship unresolved.

### org.openrewrite.kotlin.style (java)

Language style definitions and/or autodetection used by formatting; style values are separate from the visitor that applies them.

production; 9 source files. Surface: `Autodetect`, `BlankLinesStyle`, `ImportLayoutStyle`, `IntelliJ`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/java/org/openrewrite/kotlin/style/OtherStyle.java); [package annotations](../../rewrite-kotlin/src/main/java/org/openrewrite/kotlin/style/package-info.java); [test-source reference](../../rewrite-kotlin/src/test/java/org/openrewrite/kotlin/style/AutodetectTest.java).

### org.openrewrite.kotlin.table (java)

Data-table schemas emitted by the module's recipes and analysis; actual row retention/output depends on the configured DataTableStore.

production; 2 source files. Surface: `KotlinSourceFile`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/java/org/openrewrite/kotlin/table/KotlinSourceFile.java); [package annotations](../../rewrite-kotlin/src/main/java/org/openrewrite/kotlin/table/package-info.java); test relationship unresolved.

### org.openrewrite.kotlin.tree (java)

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 7 source files. Surface: `K`, `KContainer`, `KLeftPadded`, `KRightPadded`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/java/org/openrewrite/kotlin/tree/KLeftPadded.java); [package annotations](../../rewrite-kotlin/src/main/java/org/openrewrite/kotlin/tree/package-info.java); [test-source reference](../../rewrite-kotlin/src/test/java/org/openrewrite/kotlin/tree/KDocTest.java).

### org.openrewrite (kt)

Public Kotlin recipe/recipes DSL, phase builders and composite recipe classes; imperative visitors and compiler-rewritten declarative templates have different plugin requirements.

production; 1 source files. Surface: `RecipeDsl`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/kotlin/org/openrewrite/RecipeDsl.kt); [test-source reference](../../rewrite-kotlin/src/test/kotlin/org/openrewrite/RecipeDslSurfaceTest.kt).

### org.openrewrite.dsl.scopes (kt)

Recipe DSL language-visitor registration and dispatch; KotlinScope is handwritten and other scopes are generated into the build directory.

production; 2 source files. Surface: `KotlinScope`, `LanguageScope`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/kotlin/org/openrewrite/dsl/scopes/LanguageScope.kt); test relationship unresolved.

### org.openrewrite.kotlin (kt)

Contains KotlinParser/KotlinVisitor/K trees, compiler-backed type mapping, formatting and templates, plus a Kotlin recipe DSL and K2 compiler-plugin support. buildSrc generates 13 cross-language visitor scopes from compiled visitor classes; KotlinScope is handwritten. Cross-language scope dependencies are compileOnly, so recipe consumers must add the language modules they use. Gradle scope generation is intentionally absent to avoid a project cycle. The build pins Kotlin 2.4.20, uses JDK 21 and targets JVM 1.8 for main Kotlin output.

production; 3 source files. Surface: `HotPath`, `KotlinTypeMapping`, `KotlinTypeSignatureBuilder`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/kotlin/org/openrewrite/kotlin/HotPath.kt); [test-source reference](../../rewrite-kotlin/src/test/java/org/openrewrite/kotlin/FindKotlinSourcesTest.java).

### org.openrewrite.kotlin.internal (kt)

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 7 source files. Surface: `FirResolvedQualifiers`, `IrTreeVisitor`, `JavaThrownExceptions`, `JvmFacades`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/kotlin/org/openrewrite/kotlin/internal/IrTreeVisitor.kt); test relationship unresolved.

### org.openrewrite.kotlin.recipe.internal (kt)

Kotlin FIR/IR compiler-plugin registration, validation and code generation for recipe declarations; compiler-version alignment is a build-time constraint.

production; 8 source files. Surface: `Ir24ParameterCompat`, `MappedTypeFallbackBodyGenerator`, `RecipeCompilerPluginRegistrar`, `RecipeFirDslCheckers`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-kotlin/src/main/kotlin/org/openrewrite/kotlin/recipe/internal/RecipeFirExtensionRegistrar.kt); [test-source reference](../../rewrite-kotlin/src/test/kotlin/org/openrewrite/kotlin/recipe/internal/Ir24ParameterCompatTest.kt).

## rewrite-maven

### org.openrewrite.maven

MavenParser parses POMs as XML plus resolution markers, active profiles and dependency data. Cache, downloader, repository/settings and graph packages support resolving dependency relationships; recipes modify that model and XML. Resolution can involve network and local caches, so parser work is not necessarily side-effect-free. JavaRewriteRpc in this module is also the standalone Java recipe RPC server used by native-language integrations.

production; 77 source files. Surface: `AddAnnotationProcessor`, `AddCommentToMavenDependency`, `AddDependency`, `AddDependencyVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/MavenParser.java); [package annotations](../../rewrite-maven/src/main/java/org/openrewrite/maven/package-info.java); [test-source reference](../../rewrite-maven/src/test/java/org/openrewrite/maven/package-info.java).

### org.openrewrite.maven.attributes

Serializable string-keyed metadata describing external build-system attributes.

production; 2 source files. Surface: `Attribute`, `Attributed`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/attributes/Attribute.java); test relationship unresolved.

### org.openrewrite.maven.cache

POM/artifact cache implementations; read-only, in-memory and filesystem-backed variants have different persistence effects.

production; 7 source files. Surface: `CompositeMavenPomCache`, `InMemoryMavenPomCache`, `LocalMavenArtifactCache`, `MavenArtifactCache`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/cache/ReadOnlyLocalMavenArtifactCache.java); [test-source reference](../../rewrite-maven/src/test/java/org/openrewrite/maven/cache/RocksdbMavenCacheTest.java).

### org.openrewrite.maven.cleanup

Focused cleanup transformations for this language's syntax; each Recipe owns its own preconditions and before/after semantics.

production; 7 source files. Surface: `AddProjectBuildOutputTimestamp`, `DependencyManagementDependencyRequiresVersion`, `ExplicitDependencyVersion`, `ExplicitPluginGroupId`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/cleanup/DependencyManagementDependencyRequiresVersion.java); [package annotations](../../rewrite-maven/src/main/java/org/openrewrite/maven/cleanup/package-info.java); [test-source reference](../../rewrite-maven/src/test/java/org/openrewrite/maven/cleanup/ManagedDependencyRequiresVersionTest.java).

### org.openrewrite.maven.graph

Inverse dependency graphs rooted at a selected coordinate, retaining shared nodes and paths back to direct dependencies.

production; 2 source files. Surface: `DependencyGraph`, `DependencyTreeWalker`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/graph/DependencyGraph.java); [test-source reference](../../rewrite-maven/src/test/java/org/openrewrite/maven/graph/DependencyTreeWalkerTest.java).

### org.openrewrite.maven.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 12 source files. Surface: `DownloadOperation`, `InsertDependencyComparator`, `MavenParsingException`, `MavenPomDownloader`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/internal/DownloadOperation.java); [package annotations](../../rewrite-maven/src/main/java/org/openrewrite/maven/internal/package-info.java); [test-source reference](../../rewrite-maven/src/test/java/org/openrewrite/maven/internal/package-info.java).

### org.openrewrite.maven.internal.grammar

Generated grammar/token/parser interfaces and automata, sometimes alongside handwritten base helpers. Parser visitors consume these structures; use the owning grammar/generation workflow and preserve handwritten helpers.

generated; 6 source files. Surface: `VersionRangeLexer`, `VersionRangeParser`, `VersionRangeParserBaseListener`, `VersionRangeParserBaseVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/internal/grammar/VersionRangeParser.java); test relationship unresolved.

### org.openrewrite.maven.marker

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 1 source files. Surface: `AlreadyIncremented`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/marker/AlreadyIncremented.java); test relationship unresolved.

### org.openrewrite.maven.marketplace

Language-package recipe bundle discovery/resolution that bridges listings and preparation into the owning language runtime.

production; 5 source files. Surface: `CachingMavenRecipeBundleResolver`, `MavenRecipeBundleReader`, `MavenRecipeBundleResolver`, `MavenRecipeMarketplaceGenerator`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/marketplace/MavenRecipeMarketplaceGenerator.java); [package annotations](../../rewrite-maven/src/main/java/org/openrewrite/maven/marketplace/package-info.java); [test-source reference](../../rewrite-maven/src/test/java/org/openrewrite/maven/marketplace/CachingMavenRecipeBundleResolverTest.java).

### org.openrewrite.maven.plugin

Maven-plugin configuration/goal recipes, including dependency-plugin source-resolution configuration.

production; 1 source files. Surface: `DependencyPluginGoalResolveSources`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/plugin/DependencyPluginGoalResolveSources.java); [test-source reference](../../rewrite-maven/src/test/java/org/openrewrite/maven/plugin/DependencyPluginGoalResolveSourcesTest.java).

### org.openrewrite.maven.rpc

Standalone Java recipe RPC server and integration support; JavaRewriteRpc communicates over standard input/output.

production; 1 source files. Surface: `JavaRewriteRpc`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/rpc/JavaRewriteRpc.java); test relationship unresolved.

### org.openrewrite.maven.search

Search/precondition recipes that select syntax or dependency findings; result markers/tables are distinct from rewriting matched source.

production; 18 source files. Surface: `DependencyInsight`, `DoesNotIncludeDependency`, `EffectiveDependencies`, `EffectiveManagedDependencies`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/search/EffectiveMavenRepositoriesTable.java); [package annotations](../../rewrite-maven/src/main/java/org/openrewrite/maven/search/package-info.java); [test-source reference](../../rewrite-maven/src/test/java/org/openrewrite/maven/search/FindMavenProjectTest.java).

### org.openrewrite.maven.security

Recipes for repository/suppression-file security configuration. The recipe name is not a general security-audit guarantee.

production; 2 source files. Surface: `UseHttpsForRepositories`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/security/UseHttpsForRepositories.java); [package annotations](../../rewrite-maven/src/main/java/org/openrewrite/maven/security/package-info.java); [test-source reference](../../rewrite-maven/src/test/java/org/openrewrite/maven/security/UseHttpsForRepositoriesTest.java).

### org.openrewrite.maven.table

Data-table schemas emitted by the module's recipes and analysis; actual row retention/output depends on the configured DataTableStore.

production; 11 source files. Surface: `DependenciesDeclared`, `DependenciesInUse`, `DependencyGraph`, `DependencyResolutions`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/table/EffectiveMavenSettings.java); test relationship unresolved.

### org.openrewrite.maven.trait

Cursor-backed semantic views and matchers for language-specific constructs or references; use specific Matcher types where the Traits facade is deprecated.

production; 5 source files. Surface: `MavenDependency`, `MavenPlugin`, `MavenTraitMatcher`, `Traits`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/trait/Traits.java); [package annotations](../../rewrite-maven/src/main/java/org/openrewrite/maven/trait/package-info.java); [test-source reference](../../rewrite-maven/src/test/java/org/openrewrite/maven/trait/MavenDependencyTest.java).

### org.openrewrite.maven.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 26 source files. Surface: `Dependency`, `DependencyNotation`, `GroupArtifact`, `GroupArtifactClassifierType`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/tree/GroupArtifactClassifierType.java); [package annotations](../../rewrite-maven/src/main/java/org/openrewrite/maven/tree/package-info.java); [test-source reference](../../rewrite-maven/src/test/java/org/openrewrite/maven/tree/ScopeTest.java).

### org.openrewrite.maven.utilities

Recipe utilities for dependency versions and GAV validation; optional blank entries and invalid coordinates follow the source contract.

production; 6 source files. Surface: `MavenArtifactDownloader`, `MavenDependencyPropertyUsageOverlap`, `MavenWrapper`, `PrintMavenAsDot`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-maven/src/main/java/org/openrewrite/maven/utilities/RetainVersions.java); [package annotations](../../rewrite-maven/src/main/java/org/openrewrite/maven/utilities/package-info.java); [test-source reference](../../rewrite-maven/src/test/java/org/openrewrite/maven/utilities/PrintMavenAsDotTest.java).

## rewrite-ruby

### org.openrewrite.ruby

RubyParser uses the Prism WASM/API packages directly rather than starting JRuby. Prism byte offsets are translated through PrismSource, then RubyParserVisitor builds J/Rb nodes with printing markers. A shared Prism instance is synchronized because parsing is not reentrant. Ruby requires Java 21; syntax that cannot be mapped is reported rather than intentionally downgraded to J.Unknown. Corpus tests are opt-in and distinct from ordinary unit tests.

production; 6 source files. Surface: `Assertions`, `RubyIsoVisitor`, `RubyParser`, `RubyParserVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-ruby/src/main/java/org/openrewrite/ruby/RubyParser.java); [package annotations](../../rewrite-ruby/src/main/java/org/openrewrite/ruby/package-info.java); [test-source reference](../../rewrite-ruby/src/test/java/org/openrewrite/ruby/RubyParserTest.java).

### org.openrewrite.ruby.internal

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 4 source files. Surface: `PrismSource`, `RubyPrinter`, `StringUtils`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-ruby/src/main/java/org/openrewrite/ruby/internal/StringUtils.java); [package annotations](../../rewrite-ruby/src/main/java/org/openrewrite/ruby/internal/package-info.java); [test-source reference](../../rewrite-ruby/src/test/java/org/openrewrite/ruby/internal/StringUtilsTest.java).

### org.openrewrite.ruby.marker

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 16 source files. Surface: `CharacterLiteral`, `Colon2`, `EnglishOperator`, `ExplicitBegin`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-ruby/src/main/java/org/openrewrite/ruby/marker/SafeNavigation.java); [package annotations](../../rewrite-ruby/src/main/java/org/openrewrite/ruby/marker/package-info.java); test relationship unresolved.

### org.openrewrite.ruby.migrate

Language migration recipes with specific syntax transformations; supported cases are established by each recipe and its tests.

production; 2 source files. Surface: `CharacterLiteralToString`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-ruby/src/main/java/org/openrewrite/ruby/migrate/CharacterLiteralToString.java); [package annotations](../../rewrite-ruby/src/main/java/org/openrewrite/ruby/migrate/package-info.java); [test-source reference](../../rewrite-ruby/src/test/java/org/openrewrite/ruby/migrate/CharacterLiteralToStringTest.java).

### org.openrewrite.ruby.service

Language-specific service adapters for comments, imports, naming, formatting or validation; inspect the concrete service's effects before calling it.

production; 3 source files. Surface: `RubyCommentService`, `RubyWhitespaceValidationService`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-ruby/src/main/java/org/openrewrite/ruby/service/RubyWhitespaceValidationService.java); [package annotations](../../rewrite-ruby/src/main/java/org/openrewrite/ruby/service/package-info.java); [test-source reference](../../rewrite-ruby/src/test/java/org/openrewrite/ruby/service/RubyCommentServiceTest.java).

### org.openrewrite.ruby.tree

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 7 source files. Surface: `Rb`, `RubyContainer`, `RubyLeftPadded`, `RubyRightPadded`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-ruby/src/main/java/org/openrewrite/ruby/tree/RubyLeftPadded.java); [package annotations](../../rewrite-ruby/src/main/java/org/openrewrite/ruby/tree/package-info.java); [test-source reference](../../rewrite-ruby/src/test/java/org/openrewrite/ruby/tree/IdentifierTest.java).

## rewrite-scala

### org.openrewrite.scala (java)

ScalaParser and the Java-side visitors/printer work with a Scala 3 compiler bridge, type-signature builder and S nodes layered on J. Java and Scala files are jointly compiled by compileScala; compileJava excludes Java sources already handled there. The build filters Scala prereleases and launches Scala compilation with Java 17. Existing README and CLAUDE.md explain syntax mapping; their historical corpus claims are not fresh test results.

production; 10 source files. Surface: `Assertions`, `ScalaIsoVisitor`, `ScalaParser`, `ScalaParserVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-scala/src/main/java/org/openrewrite/scala/ScalaParser.java); [package annotations](../../rewrite-scala/src/main/java/org/openrewrite/scala/package-info.java); [test-source reference](../../rewrite-scala/src/test/java/org/openrewrite/scala/ScalaVisitorTest.java).

### org.openrewrite.scala.format (java)

Formatting recipes and visitors that adjust spacing, indentation or line breaks in the language model.

production; 4 source files. Surface: `AutoFormat`, `AutoFormatVisitor`, `MergeSpacesVisitor`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-scala/src/main/java/org/openrewrite/scala/format/AutoFormat.java); [package annotations](../../rewrite-scala/src/main/java/org/openrewrite/scala/format/package-info.java); [test-source reference](../../rewrite-scala/src/test/java/org/openrewrite/scala/format/MergeSpacesVisitorTest.java).

### org.openrewrite.scala.internal (java)

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 4 source files. Surface: `ScalaCompilerContext`, `ScalaSpace`, `ScalaTryBuilder`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-scala/src/main/java/org/openrewrite/scala/internal/ScalaSpace.java); [package annotations](../../rewrite-scala/src/main/java/org/openrewrite/scala/internal/package-info.java); [test-source reference](../../rewrite-scala/src/test/scala/org/openrewrite/scala/internal/BinaryExpressionDebug.scala).

### org.openrewrite.scala.internal.template (java)

Internal snippet/template parsing, substitutions and generated context support; consumed by the language's public template layer.

production; 4 source files. Surface: `ScalaAnnotationTemplateGenerator`, `ScalaBlockStatementTemplateGenerator`, `ScalaSubstitutions`, `ScalaTemplateParser`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-scala/src/main/java/org/openrewrite/scala/internal/template/ScalaTemplateParser.java); test relationship unresolved.

### org.openrewrite.scala.marker (java)

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 23 source files. Surface: `AmpersandIntersection`, `AsInstanceOfPrefix`, `BlockArgument`, `CommaContinuation`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-scala/src/main/java/org/openrewrite/scala/marker/UnderscorePlaceholderLambda.java); test relationship unresolved.

### org.openrewrite.scala.service (java)

Language-specific service adapters for comments, imports, naming, formatting or validation; inspect the concrete service's effects before calling it.

production; 2 source files. Surface: `ScalaAutoFormatService`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-scala/src/main/java/org/openrewrite/scala/service/ScalaAutoFormatService.java); [package annotations](../../rewrite-scala/src/main/java/org/openrewrite/scala/service/package-info.java); test relationship unresolved.

### org.openrewrite.scala.style (java)

Language style definitions and/or autodetection used by formatting; style values are separate from the visitor that applies them.

production; 2 source files. Surface: `Autodetect`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-scala/src/main/java/org/openrewrite/scala/style/Autodetect.java); [package annotations](../../rewrite-scala/src/main/java/org/openrewrite/scala/style/package-info.java); test relationship unresolved.

### org.openrewrite.scala.tree (java)

Language-specific tree nodes, interfaces, spacing and padding values used by parser, visitors, printer and recipes. These shapes are the transformation model, not generated source output.

production; 2 source files. Surface: `S`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-scala/src/main/java/org/openrewrite/scala/tree/S.java); [package annotations](../../rewrite-scala/src/main/java/org/openrewrite/scala/tree/package-info.java); [test-source reference](../../rewrite-scala/src/test/java/org/openrewrite/scala/tree/SingletonTypeTreeTest.java).

### org.openrewrite.scala.internal (scala)

Parser/printer, type-mapping and other implementation helpers used by the public language surface; individual errors and resource effects remain in the linked source.

production; 5 source files. Surface: `ScalaASTConverter`, `ScalaCompilerBridge`, `ScalaTreeVisitor`, `ScalaTypeMapping`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-scala/src/main/scala/org/openrewrite/scala/internal/ScalaTypeSignatureBuilder.scala); [test-source reference](../../rewrite-scala/src/test/scala/org/openrewrite/scala/internal/BinaryExpressionDebug.scala).

### org.openrewrite.scala.marker (scala)

Language/build metadata and printing distinctions carried alongside syntax, rather than replacing the tree's child structure.

production; 1 source files. Surface: `ScalaMarkers`. [Source](https://github.com/hsoliwal/rewrite/blob/0c82f71bc8bb215a8194db59a3784551d6a60e99/rewrite-scala/src/main/scala/org/openrewrite/scala/marker/ScalaMarkers.scala); test relationship unresolved.

