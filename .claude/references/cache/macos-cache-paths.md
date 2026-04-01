# macOS Cache Paths

| PM | Primary Path | Secondary Path | Env Override | File Pattern | Extension |
|---|---|---|---|---|---|
| Cargo | `~/.cargo/registry/cache/{registry-hash}/` | | `CARGO_HOME` | `{name}-{version}.crate` | `.crate` |
| Poetry | `~/Library/Caches/pypoetry/artifacts/` | `~/.cache/pypoetry/` | `POETRY_CACHE_DIR` | `{name}-{version}*.whl` | `.whl`, `.tar.gz` |
| Helm | `~/Library/Caches/helm/repository/` | `~/.cache/helm/repository/` | `HELM_REPOSITORY_CACHE` | `{name}-{version}.tgz` | `.tgz` |
| Maven | `~/.m2/repository/{groupPath}/{artifactId}/{version}/` | | `M2_HOME` | `{artifactId}-{version}.jar` | `.jar`, `.war`, `.pom` |
| Gradle | `~/.gradle/caches/modules-2/files-2.1/{group}/{artifact}/{version}/{hash}/` | | `GRADLE_USER_HOME` | `{artifact}-{version}.jar` | `.jar`, `.aar` |
| Conan | `~/.conan2/p/{pkgname}{hash}/` | | `CONAN_HOME` | archives in export dir | `.tgz`, `.tar.gz` |
| npm | `~/.npm/_cacache/content-v2/` | | `npm_config_cache` | content-addressable | N/A |
| pnpm | `~/Library/pnpm/store/v3/` | | `PNPM_HOME` | NO tarballs (Non-FlexPack) | N/A |
| Go | `~/go/pkg/mod/cache/download/` | | `GOPATH`, `GOMODCACHE` | `{module}/@v/{version}.zip` | `.zip` |
| RubyGems | `~/.gem/ruby/{ruby-version}/cache/` | | `GEM_HOME` | `{name}-{version}.gem` | `.gem` |

**Notes**: Cargo has NO `v` prefix. Go uses `v` prefix. Conan requires `conan cache path`. Gradle has hash subdirectory.
