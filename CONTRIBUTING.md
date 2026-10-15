# Contributing to this OpenRewrite fork

This is [hsoliwal/rewrite](https://github.com/hsoliwal/rewrite), a fork of
[openrewrite/rewrite](https://github.com/openrewrite/rewrite). The upstream project
and its contributor community retain their identity and attribution. The
[M3 direction in the README](README.md#m3-direction-in-this-fork) describes this
fork's experimental role; it does not establish that a recipe is qualified for a
product runtime.

For changes to this fork, target a topic PR at `hsoliwal/rewrite:main`. For an
upstream contribution, follow the
[upstream contribution guide](https://github.com/openrewrite/.github/blob/main/CONTRIBUTING.md)
and select the upstream destination deliberately. The README's upstream CI badge
reports upstream status, not the status of your fork commit.

## Find the right module

The [architecture and coverage guide](doc/architecture/README.md) extends this
quick map with all configured Gradle projects, nested native builds, package
responsibilities, selected API contracts and an honest inventory of remaining
semantic/inline-documentation work.

- [rewrite-core](rewrite-core/): shared tree, visitor and recipe infrastructure
- [rewrite-test](rewrite-test/): recipe-test support, including `RewriteTest`
- [rewrite-java](rewrite-java/): Java trees, visitors and recipes; version-specific
  parsers also live in `rewrite-java-8`, `rewrite-java-11`, `rewrite-java-17`,
  `rewrite-java-21` and `rewrite-java-25`
- [rewrite-json](rewrite-json/), [rewrite-xml](rewrite-xml/) and
  [rewrite-yaml](rewrite-yaml/): examples of format-specific parser/recipe modules
- [rewrite-maven](rewrite-maven/) and [rewrite-gradle](rewrite-gradle/): build-file
  support
- [architecture decisions](doc/adr/) and [repository development guidance](CLAUDE.md):
  design history and contribution conventions

[settings.gradle.kts](settings.gradle.kts) is the source of truth for included
projects. Language integrations can require their own tools; consult, for example,
the [Python instructions](rewrite-python/rewrite/CLAUDE.md) and
[JavaScript/TypeScript instructions](rewrite-javascript/rewrite/CLAUDE.md) before
running those suites. Their RPC-based tests need bounded timeouts and focused test
selection, as described in the repository guidance.

## Prerequisites and first inspection

Use the checked-in wrapper: `./gradlew` on a POSIX shell, or `gradlew.bat` on
Windows. [The wrapper configuration](gradle/wrapper/gradle-wrapper.properties)
currently pins Gradle 9.7.0 and a distribution checksum. A first run needs access
to the configured distribution and dependency repositories.

Have JDK 21 available for Java development. The
[Java module](rewrite-java/build.gradle.kts) requests a Java 21 toolchain and the
[Java 25 parser](rewrite-java-25/build.gradle.kts) requests Java 25. Other modules
have their own requirements: one launcher JDK does not satisfy every compiler
toolchain automatically. The [CI workflow](.github/workflows/ci.yml) requests
Java 25 and 21 through an upstream reusable workflow.

Run from the repository root:

```sh
./gradlew --version
./gradlew projects
./gradlew :rewrite-json:tasks --all
```

Check the JVM shown by `--version`, then provision any additional toolchains
required by your selected modules. Gradle/plugin resolution may fail before a
task starts. [settings.gradle.kts](settings.gradle.kts) declares the plugin
repositories and optional Codegenome credentials; report the repository and
resolution error without putting credentials in a PR or log.

The build uses dynamic plugin/dependency versions such as `latest.release`.
Record the resolved versions and exact commit when sharing a reproduction.
[gradle.properties](gradle.properties) enables parallel builds, caps workers at
six and gives the Gradle JVM a 3 GiB heap. Those settings are not a total-memory
budget for compilers and test JVMs.

## Start with a focused check

The checked-in
[JsonParserTest](rewrite-json/src/test/java/org/openrewrite/json/JsonParserTest.java)
is a concrete example using `RewriteTest`:

```sh
# One existing test class
./gradlew :rewrite-json:test --tests "org.openrewrite.json.JsonParserTest"

# All tests in the affected module
./gradlew :rewrite-json:test

# Broader checks after the focused change is working
./gradlew check
./gradlew build
```

Choose the corresponding module and existing test class for your change.
`assemble` is useful for compilation/packaging, but is not a test pass. A focused
parser test is not a full multi-language or integration-suite result. In
particular, `rewrite-java-25:check` also includes its declared
`compatibilityTest` suite; running only `test` does not cover that suite.

For recipe changes, include before/after examples and unchanged-input cases.
Preserve formatting, comments and type information; the repository guidance
prohibits downgrading already modeled syntax to `J.Unknown`. For generated
parser changes, inspect that module's `generateAntlrSources` task and its grammar
inputs rather than editing generated output without the owning workflow.

### IDE imports

An optional local `IDE.properties` file selects projects only when
`idea.active` or `idea.sync.active` is set. Its keys are module names;
`settings.gradle.kts` always includes the two Gradle tooling-model projects.
This is an IDE import aid, not a command-line switch to make the normal build
exclude modules.

## Licenses and review evidence

Read the [root LICENSE](LICENSE), the license of each affected module, and
existing source headers before copying or moving material. Some language modules
carry separate terms, for example
[rewrite-javascript/LICENSE.md](rewrite-javascript/LICENSE.md).
The root Apache-2.0 license must not be presented as a blanket license for every
module. The [M3/Synexia rights notice](M3-SYNEXIA-RIGHTS.md) preserves the boundary
between first-party additions and upstream work.

In the PR, include:

- the purpose, changed modules, base/head commits and intended fork or upstream target
- exact commands, JDK/OS, resolved toolchain details and the scope of tests run
- separate passed, failed, skipped, blocked and not-run results
- relevant source/recipe provenance and preserved license/change notices
- remaining parser, integration or platform coverage

These commands are derived from the checked-in build and contributor guidance.
This documentation change did not execute Gradle, tests, code generation or
benchmarks; it is not evidence that the current source passes those checks.
