# OpenRewrite contributor learning guide

Use this guide to navigate this fork's source and follow one recipe through its
tests. For running a published recipe on an application, begin with the
[upstream quickstart linked in the README](../README.md). Building this
multi-language engine is a different task from applying a recipe to a project.

## Vocabulary that makes the source easier to read

| Term | Practical meaning |
| --- | --- |
| Recipe | A named transformation with configuration, validation, and a visitor |
| Lossless Semantic Tree (LST) | Parsed source retaining formatting, comments, and semantic/type information |
| Visitor | Traversal that returns unchanged or replacement nodes; follow returned values rather than mutating nodes in place |
| Type attribution | Knowledge of the referenced types/methods, allowing semantic matching rather than text replacement |
| Marker | Metadata associated with a tree node |
| RewriteTest | Test harness expressing source before a recipe and expected source afterward |

The central pipeline is source → parser → LST → recipe/visitor → changed LST →
printed source. A plausible text diff alone does not prove correct type metadata
or behavior. Preserve rich node types and inspect the tests that verify them.

## Where to start

- [rewrite-core](../rewrite-core): recipe, visitor, source-file, and execution infrastructure.
- [rewrite-java](../rewrite-java): Java trees, matching, visitors, and recipes.
- [rewrite-java-test](../rewrite-java-test): Java recipe test cases, including
  the worked example below. Do not assume the implementation and its tests live
  in the same module.
- [rewrite-test](../rewrite-test): shared recipe-testing infrastructure.
- [rewrite-java-21](../rewrite-java-21) and other Java-version modules: parser
  implementations/toolchain concerns distinct from individual recipe logic.
- [doc/adr](adr): architecture decisions; [CLAUDE.md](../CLAUDE.md) records
  repository conventions and pointers to language-specific instructions.
- [settings.gradle.kts](../settings.gradle.kts): included modules and IDE-only
  selective loading; [build.gradle.kts](../build.gradle.kts): shared build setup.

Python, JavaScript, C#, Ruby, and Scala have additional module instructions.
Read the appropriate module's guidance before assuming the Java workflow covers
its external runtime, package manager, or RPC tests.

## Prerequisites and command cheat sheet

Use the checked-in Gradle wrapper and a JDK. The checked-in CI configuration
requests JDKs 25 and 21; the worked example's
[rewrite-java-test build](../rewrite-java-test/build.gradle.kts) explicitly
selects a Java 21 toolchain. Make that toolchain available and check the selected
module's requirements rather than assuming one JDK suffices for every parser.
On Windows use `gradlew.bat` instead of `./gradlew`.

Commands below were source-checked, **not run** for this guide:

```bash
# Inspect the actual wrapper/JVM combination and included projects.
./gradlew --version
./gradlew projects

# Focus on the recipe test class used below.
./gradlew :rewrite-java-test:test --tests 'org.openrewrite.java.ChangeMethodNameTest'

# Wider checks, after the focused test succeeds.
./gradlew :rewrite-java-test:test
./gradlew assemble
./gradlew check
```

`assemble` is not a substitute for tests. Whole-repository `check` can be much
broader than one language or recipe. Dependency downloads and the configured
plugin repositories must be reachable; settings include an authenticated
Codegenome repository and moving `latest.release` plugin versions. If resolution
fails, report the actual dependency/repository error before diagnosing recipe
logic. Never put credentials into a patch or documentation example.

`IDE.properties` filtering is activated by IDE-related system properties in
settings. Creating that file alone does not reduce an ordinary command-line
build. For RPC-based language tests, follow the documented explicit timeout and
small-test-scope guidance in `CLAUDE.md`.

## Worked example: rename one method semantically

Open these two source files side by side:

- [ChangeMethodName.java](../rewrite-java/src/main/java/org/openrewrite/java/ChangeMethodName.java)
- [ChangeMethodNameTest.java](../rewrite-java-test/src/test/java/org/openrewrite/java/ChangeMethodNameTest.java)

### 1. Read the contract before the traversal

The recipe accepts a method pattern, new name, and optional override/definition
controls. `validate()` checks the method pattern and rejects reserved keywords,
reserved literals, and invalid Java identifiers. Renaming is not a global search
and replace of every matching word.

### 2. Follow the existing smallest example

`changeMethodNameForMethodWithSingleArg()` configures the pattern
`com.abc.B singleArg(String)` and the replacement `bar`. Its input call
`new B().singleArg("boo")` is expected to become `new B().bar("boo")`.
The supporting class B supplies method attribution and is passed with
`SourceSpec::skip`; inspect that fixture role before assuming it is another
asserted output file.

### 3. Find both the precondition and the mutation

`getVisitor()` builds a condition using `DeclaresMethod` and `UsesMethod`, then
wraps the actual visitor in `Preconditions.check`. The actual traversal handles
declarations, invocations, member references, and static imports separately.
Read `visitMethodInvocation`: matching calls update both their visible name and
`JavaType.Method` name when type information is present.

This explains why changing only printed text is insufficient: a later recipe
can inspect the semantic method name even if the source already looks correct.

### 4. Expand from the happy path

`changeMethodNameForOverriddenMethod()` enables override matching and checks a
call plus a method reference. Its `afterRecipe` assertions inspect method-type
names as well as source text. Nearby tests cover disabled override matching and
anonymous classes. These are useful patterns for choosing regression cases.

Before extending the recipe, write down the target signature, override behavior,
definition behavior, an input that should change, and an input that must remain
unchanged. Then add a focused `RewriteTest` case and run the class above.

## Troubleshooting and review checklist

| Symptom | First place to investigate |
| --- | --- |
| No tests matched | Test class/module location; this example is in `rewrite-java-test` |
| A matching-looking call stays unchanged | Method pattern, parser attribution/classpath, preconditions, and override settings |
| Printed output is right but later transformations fail | Semantic types/markers and `afterRecipe` assertions |
| Unknown node introduced | Repository rule against replacing an already rich type with `J.Unknown` |
| Test hangs in a non-Java language | Its RPC/runtime setup and timeout guidance before enlarging the test scope |

Keep recipe changes reviewable: before/after examples, negative cases, preserved
comments/formatting, semantic assertions where relevant, and exact checks run.
The fork's research direction does not establish that a particular recipe has
been integrated, benchmarked, or behaviorally validated. Existing upstream
authorship and [license](../LICENSE) remain authoritative.
