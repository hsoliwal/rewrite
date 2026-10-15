<!-- fork-homepage-guide:start -->
# Learn and extend repeatable source transformations

This [hsoliwal fork of OpenRewrite](https://github.com/hsoliwal/rewrite) is a
place to study parsers, semantic trees, and reusable refactoring recipes. The
[upstream engine](https://github.com/openrewrite/rewrite) supplies the broad
language and transformation foundation. Use this checkout when developing or
investigating the engine; applying a published recipe to your application is a
separate workflow covered by the upstream quickstart below.

## Implementation and fork direction

The tree contains parsers, recipe/visitor APIs, test harnesses and language
modules. Fork-specific M3 material includes a small bootstrap recipe sidecar and
analysis tooling, described in [.m3/README.md](.m3/README.md) and the
[architecture guide](doc/architecture/README.md). The wider reusable-computation
programme is a direction, not proof that every proposed transformation, semantic
check, or runtime optimization is implemented or qualified. This README change
was source-reviewed; it did not run Gradle, recipes, tests, or benchmarks.

## The concepts behind a recipe

| Term | Why it matters |
| --- | --- |
| Recipe | A named transformation with configuration, validation and traversal logic |
| Lossless Semantic Tree (LST) | Source structure retaining comments, formatting and semantic/type information |
| Visitor | A traversal returning unchanged or replacement nodes |
| Type attribution | Lets a recipe target a particular method/type instead of every matching word |
| Marker | Metadata attached to a tree node |
| `RewriteTest` | Before/after and unchanged-source assertions for recipe behavior |

The flow is **source → parser → LST → recipe/visitor → changed LST → printed
source**. A review should examine semantic correctness as well as the printed
diff. Shared APIs live in [`rewrite-core`](rewrite-core); Java recipes in
[`rewrite-java`](rewrite-java); many Java recipe tests live separately in
[`rewrite-java-test`](rewrite-java-test).

## Quickstart: one focused recipe test

Use the checked-in Gradle wrapper. The example test module selects a Java 21
toolchain; other modules and CI have additional JDK/runtime requirements. Follow
[CONTRIBUTING.md](CONTRIBUTING.md) before a whole-repository build.

```sh
git clone https://github.com/hsoliwal/rewrite.git
cd rewrite
./gradlew --version
./gradlew :rewrite-java-test:test --tests 'org.openrewrite.java.ChangeMethodNameTest'
```

On Windows, use `gradlew.bat`. Dependency/plugin resolution must work first;
settings include moving plugin versions and an authenticated repository. A
resolution failure does not diagnose recipe correctness. Record the source SHA,
JDKs and resolved versions when sharing a reproduction; never commit credentials.

## Worked example: rename a method, not a word

[`ChangeMethodName`](rewrite-java/src/main/java/org/openrewrite/java/ChangeMethodName.java)
accepts a method signature pattern and replacement name. The existing
[`ChangeMethodNameTest`](rewrite-java-test/src/test/java/org/openrewrite/java/ChangeMethodNameTest.java)
uses `com.abc.B singleArg(String)` with replacement `bar`:

```java
// Before                         // Expected after
new B().singleArg("boo");          new B().bar("boo");
```

The fixture supplies B's type so the recipe can match it semantically. Unrelated
methods with the same spelling should stay unchanged. Override matching and
`ignoreDefinition` are separate controls; the latter participates in a whole
source-file precondition, so inspect a definition-plus-usage fixture before
assuming it skips only declaration nodes. Preserve comments, formatting, and
method-type metadata, and assert unchanged cases alongside changed cases.

## Compact cheat sheet

| Goal | Command or source | Scope |
| --- | --- | --- |
| Discover projects | `./gradlew projects` | Actual configured modules, not just directory names |
| Run the example | Focused `ChangeMethodNameTest` command above | One recipe test class |
| Broaden Java recipe checks | `./gradlew :rewrite-java-test:test` | That test module |
| Compile/package | `./gradlew assemble` | Not a test result |
| Broaden validation | `./gradlew check` | Wider toolchain/runtime requirements apply |
| Author a reusable change | Recipe + positive/negative `RewriteTest` fixtures | Review output and semantic contracts before application |

For a fuller recipe lab, see the [contributor learning guide](doc/CONTRIBUTOR_GUIDE.md).
For module and API responsibilities, see [architecture and coverage](doc/architecture/README.md).
Keep recipes reusable and qualified before applying them to products. Upstream
project text follows intact; consult root and per-module licenses, since one
root license does not describe every language module's terms.
<!-- fork-homepage-guide:end -->

---

## Explore this fork

Start with the [contributor learning guide and command cheat sheet](doc/CONTRIBUTOR_GUIDE.md) for the recipe vocabulary, a worked semantic rename, focused tests, and troubleshooting.

## M3 direction in this fork

M3 is based on the idea that shared immutable structure and indexed metadata can
provide a basis for reusable computation. The programme starts with M3 String,
regex, and precompute in M3JDK21, then collections and SWT/Eclipse integration.

This OpenRewrite fork is an exploration surface for reusable transformation recipes: making source changes repeatable, reviewable, and governed by compiler and behavioral checks. Canonical M3 recipes converge in Synexia before product application.

**Explore the idea:** [M3 technical design paper](https://github.com/hsoliwal/M3jdk21/blob/master/m3/papers/M3_SHARED_STRUCTURE_AND_REUSABLE_COMPUTATION.md) ·
[Programme goals and roadmap](https://github.com/hsoliwal/M3jdk21#project-goals-and-plan).
The paper explains the proposed architecture, cost tradeoffs, and evaluation plan.
Readers are invited to examine the work and contribute representative workloads.

This section describes the direction of the hsoliwal fork. Upstream documentation
follows below; upstream authorship, licenses, and project identity remain intact.

## Working on this fork

Start with the [fork contributor guide](CONTRIBUTING.md) for the module map,
JDK/toolchain setup, focused tests, IDE imports and per-module license boundaries.
For the source-pinned project/module architecture, package responsibilities and
explicit documentation gaps, see the [architecture and coverage guide](doc/architecture/README.md).
The checked-in [settings](settings.gradle.kts) and
[build configuration](build.gradle.kts) describe this checkout. Propose fork
changes against `hsoliwal/rewrite:main`; upstream contribution instructions and
badges below refer to the upstream project.

---

<p align="center">
  <a href="https://docs.openrewrite.org">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/openrewrite/rewrite/raw/main/doc/logo-oss-dark.svg">
      <source media="(prefers-color-scheme: light)" srcset="https://github.com/openrewrite/rewrite/raw/main/doc/logo-oss-light.svg">
      <img alt="OpenRewrite Logo" src="https://github.com/openrewrite/rewrite/raw/main/doc/logo-oss-light.svg" width='600px'>
    </picture>
  </a>
</p>

<div align="center">

<!-- Keep the gap above this line, otherwise they won't render correctly! -->
[![ci](https://github.com/openrewrite/rewrite/actions/workflows/ci.yml/badge.svg)](https://github.com/openrewrite/rewrite/actions/workflows/ci.yml)
[![Apache 2.0](https://img.shields.io/github/license/openrewrite/rewrite.svg)](https://www.apache.org/licenses/LICENSE-2.0)
[![Revved up by Develocity](https://img.shields.io/badge/Revved%20up%20by-Develocity-06A0CE?logo=Gradle&labelColor=02303A)](https://community.develocity.cloud/scans?search.rootProjectNames=rewrite)
[![Contributing Guide](https://img.shields.io/badge/Contributing-Guide-informational)](https://github.com/openrewrite/.github/blob/main/CONTRIBUTING.md)
</div>

<h1 align="center">Fast, repeatable refactoring for developers</h1>

The OpenRewrite project (managed by [Moderne](https://www.moderne.ai/)) is an open-source automated refactoring ecosystem for source code, enabling developers to effectively eliminate technical debt within their repositories.

It consists of an auto-refactoring engine that runs prepackaged, open source refactoring recipes for common framework migrations, security fixes, and stylistic consistency tasks—reducing your coding effort from hours or days to minutes.

OpenRewrite provides parsers and base recipes for [multiple languages](https://docs.openrewrite.org/reference/supported-languages), including Java, Kotlin, Groovy, JavaScript/TypeScript, Python, and C#. For Java, the [OpenRewrite Gradle Plugin](https://docs.openrewrite.org/reference/gradle-plugin-configuration) and [OpenRewrite Maven Plugin](https://docs.openrewrite.org/reference/rewrite-maven-plugin) let you run one recipe at a time against a single repository. Recipes are easy to customize, so you can adapt any of them - or even write your own.

Start with our [quickstart guide](https://docs.openrewrite.org/running-recipes/getting-started) and let OpenRewrite handle the boring parts of software development for you.

Get and stay informed:
* Read the [documentation](http://docs.openrewrite.org).
* Join us on [Slack](https://join.slack.com/t/rewriteoss/shared_invite/zt-nj42n3ea-b~62rIHzb3Vo0E1APKCXEA) or [Discord](https://discord.gg/xk3ZKrhWAb)! We're happy to answer your questions directly.
* Check out [Code Remix Weekly](https://www.youtube.com/@moderne-and-openrewrite/streams) where we deep dive topics and answer questions.
* Subscribe to our [YouTube](https://www.youtube.com/@moderne-and-openrewrite) channel for great videos on OpenRewrite recipes.
* Follow us on [X](https://x.com/openrewrite) and [LinkedIn](https://www.linkedin.com/company/moderneinc).

OpenRewrite is maintained by Moderne and is offered under a tiered licensing model. See [Licensing](https://docs.openrewrite.org/licensing/openrewrite-licensing) for how the framework, parsers, and recipes are licensed.

## Building on OpenRewrite with Moderne
 
OpenRewrite is built to migrate, secure, and refactor code one repository at a time. [Moderne](https://www.moderne.ai/) is the commercial platform built on that foundation, running the same recipes across hundreds or thousands of repositories at once. Migrations are a common starting point with OpenRewrite, and the platform expands on this to include large-scale impact analysis, security remediation, and the tools coding agents use to understand and change code across a whole organization. Moderne also extends OpenRewrite from Java to additional languages and from individual developers to teams and coding agents. The parsers and base recipes for those additional languages are provided under the [Moderne Source Available License](https://docs.openrewrite.org/licensing/openrewrite-licensing#moderne-source-available-license), which lets you run them against your own code at no cost. Moderne also offers [proprietary recipes](https://docs.openrewrite.org/licensing/openrewrite-licensing#moderne-proprietary-license) in these ecosystems (for large-scale analysis, security remediation, and AI tool-calling) that require a Moderne commercial license. 
 
A lot of this comes down to how the [Lossless Semantic Tree (LST)](https://docs.openrewrite.org/concepts-and-explanations/lossless-semantic-trees) is handled. The LST is OpenRewrite's type-aware model of your source code, built in memory each time a recipe runs. Moderne batch-builds LSTs once and serializes them, so they can be reused across repos and across teams without rebuilding. That same LST gives coding agents pre-computed context, type-aware search, and direct access to OpenRewrite recipes as deterministic tool calls through tools like [Prethink](https://docs.moderne.io/user-documentation/agent-tools/prethink), [Trigrep](https://docs.moderne.io/user-documentation/agent-tools/trigrep), and a [local MCP server](https://docs.moderne.io/user-documentation/agent-tools/mcp). Agents work with less token overhead and more accuracy than they would reading a codebase file by file.
 
See how Moderne and OpenRewrite work in different ways to transform your code:

[![Moderne](./doc/openrewrite_v_moderne.png)](https://www.youtube.com/watch?v=Q-ej2lCJHRs)

Moderne also includes a [multi-repo command line interface (CLI)](https://docs.moderne.io/user-documentation/moderne-cli/getting-started/cli-intro) for building LSTs, running recipes locally, and developing custom recipes across many repositories.

[![Moderne](./doc/moderne_cli.png)](https://www.youtube.com/watch?v=fdPX9e2vsFw)

Moderne runs a free [public service](https://www.moderne.io/try-moderne) for the benefit of tens of thousands of open source projects. To learn more about how Moderne can help your team, [contact us](https://www.moderne.io/contact-us).

## Contributing

We appreciate all types of contributions. See the [contributing guide](https://github.com/openrewrite/.github/blob/main/CONTRIBUTING.md) for detailed instructions on how to get started.
