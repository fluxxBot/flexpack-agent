# Scope Type Catalog

| PM | Scopes | Source | Default |
|---|---|---|---|
| Poetry | main, dev, transitive | pyproject.toml sections | transitive |
| Cargo | runtime, dev, build, transitive | Cargo.toml sections | transitive |
| pnpm | runtime, dev, transitive | package.json deps/devDeps | transitive |
| Helm | (none — flat) | N/A | compile |
| Conan | runtime, build, test | context: host→runtime, build→build, test→test | runtime |
| Gradle | compile, runtime, test, provided, system | configuration name (see below) | compile |
| Maven | compile, runtime, test, provided, system, import | `<scope>` in POM | compile |
| Docker | (none) | N/A | N/A |
| Go | runtime, test | *_test.go imports → test | runtime |
| RubyGems | runtime, dev | Gemfile groups | runtime |

## Gradle Configuration→Scope Mapping
| Configuration | Scope |
|---|---|
| compileClasspath | compile |
| runtimeClasspath | runtime |
| testCompileClasspath, testRuntimeClasspath | test |
| debugCompileClasspath (Android) | compile |
| debugRuntimeClasspath (Android) | runtime |
| releaseCompileClasspath (Android) | compile |
| Unknown | compile (default) |
