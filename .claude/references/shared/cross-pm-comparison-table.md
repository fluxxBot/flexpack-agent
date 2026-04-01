# Cross-Package-Manager Comparison Table

| Aspect | Poetry | Cargo | pnpm | Helm | Conan | Gradle | Maven | Docker |
|---|---|---|---|---|---|---|---|---|
| **Architecture** | FlexPack | FlexPack | **Non-FlexPack** | FlexPack | FlexPack | FlexPack | FlexPack | **Non-FlexPack** |
| **Code in build-info-go** | Yes | Yes | **No** | Yes | Yes | Yes | Yes | **No** |
| **Manifest Format** | TOML (pyproject.toml) | TOML (Cargo.toml) | JSON (package.json) | YAML (Chart.yaml) | Python/INI (conanfile.py/txt) | Groovy/Kotlin DSL (build.gradle) | XML (pom.xml) | Dockerfile |
| **Lock File Format** | TOML (poetry.lock) | TOML (Cargo.lock) | YAML (pnpm-lock.yaml) | YAML (Chart.lock) | JSON (conan.lock) | **None** | **None** | **None** |
| **CLI Resolution Command** | `poetry show --tree` | `cargo tree` | `pnpm ls -r --depth Infinity --json` | `helm dependency list` | `conan graph info --format=json` | `gradle :mod:dependencies` | `mvn dependency:tree -DoutputType=json` | go-containerregistry |
| **Checksum Source** | Local .whl/.tar.gz | Local .crate files | **Server AQL + build cache** | Local .tgz files | CLI-located archives | Local .jar files | Local .jar/.war files | **Server AQL + Docker API** |
| **Multi-Module** | No | Workspace | Workspace | No | No | Yes (settings.gradle) | Yes (parent POM) | Multi-platform |
| **Scope Types** | main, dev, transitive | runtime, dev, build, transitive | runtime, dev, transitive | (none — flat) | runtime, build, test | compile, runtime, test, provided, system | compile, runtime, test, provided, system, import | (none) |
| **Dual Protocol** | No | No | No | **Yes** (Classic + OCI) | No | No | No | **Yes** (Docker + OCI) |
| **Complexity** | 2/5 | 3/5 | 3.5/5 | 3/5 | 3.5/5 | 4.5/5 | 3.5/5 | 4/5 |
| **Resolution Strategy** | Hybrid (lock + CLI) | Hybrid (lock + CLI) | CLI-only | Lock file | CLI-only | CLI-only | CLI-only | Server-only |
| **Android Support** | No | No | No | No | No | **Yes** | No | No |
