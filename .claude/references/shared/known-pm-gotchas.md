# Known Package Manager Gotchas

## Cargo
- **Version format**: Cache filenames have NO `v` prefix (`clap-4.5.41.crate`, not `clap-v4.5.41.crate`). Before fixing this: 0% cache hits. After: 98%.
- **Dependency scale**: 3 direct dependencies → 55+ total (including transitives). Design for 20-30x scale.
- **CLI output**: `cargo tree` output includes status prefixes ("Downloading:", "Downloaded:") that must be filtered.
- **Cache population timing**: Cache files aren't immediately available after `cargo build` — may need to wait or re-run.

## Poetry
- **Lock file optional**: Many Poetry projects don't commit `poetry.lock`. System must work without it.
- **CLI tree parsing**: `poetry show --tree` output uses tree characters (│, ├, └) that need careful stripping.
- **Dependency scale**: 3-4 direct deps → 50+ transitive deps in real projects.
- **Cache structure**: Well-organized in `~/.cache/pypoetry/` with predictable paths.

## pnpm
- **Metadata cache staleness**: pnpm caches package metadata globally at `~/Library/Caches/pnpm/metadata-v1.3/<host>/`. NOT controlled by `--store-dir`. In CI/test environments where Artifactory repo names change, stale metadata causes 401 errors. Fix: clear host-specific metadata cache before install.
- **`.npmrc` required**: Without project-level `.npmrc` pointing to Artifactory, pnpm resolves from `registry.npmjs.org` directly → AQL queries target empty repo → no checksums.
- **Build cache first build**: `DependenciesToChecksumMap` filters out deps where all checksums are empty. First build MUST resolve through Artifactory to populate checksums via AQL.
- **Workspace link deps**: `link:` prefixed versions are local symlinks, not real packages. Skip them: `if strings.HasPrefix(info.Version, "link:") { continue }`.
- **`go.mod` replace directives**: When `go.mod` uses remote `replace`, local changes to code are NOT used during `go test`. Use local path replace during development.
- **Publish summary gotcha**: `pnpm-publish-summary.json` must be moved/removed before `pnpm pack` on workspace packages, otherwise it gets included in the tarball.
- **Scoped artifact path**: `@scope/name/-/@scope/name-version.tgz` in Artifactory.

## Helm
- **`Chart.lock` must exist** even for charts without dependencies (tests create empty `dependencies: []`).
- **CLI auto-detection**: Looks for `helm.exe` on Windows first, then `helm`; fails fast if not in PATH.
- **Dependencies loaded once**: Lazy-loaded and never reloaded. Changes after `helm dependency update` require a new HelmFlexPack instance.
- **Chart name parsing**: `my-chart-1.0.0.tgz` — version is the last dash-separated segment. Names with dashes need careful parsing.
- **No transitive deps**: Helm dependency graph is flat (one level deep) by design.

## Conan
- **Version 2.x MINIMUM**: Code explicitly checks `conan --version >= 2.0.0`. Conan 1.x is NOT supported.
- **Cache path volatility**: Paths contain hash components that change. MUST use `conan cache path <ref>` CLI command — no shortcut.
- **Python attribute parsing**: Simple regex, not Python AST. Takes first occurrence of `name = "..."`. Handles both quote types.
- **Profile/Settings/Options affect resolution**: Must be explicitly passed to `conan graph info`. Not auto-discovered.
- **Empty version valid**: Consumer-only recipes may have no version.

## Gradle
- **Gradle Wrapper preferred**: ALWAYS check for `gradlew`/`gradlew.bat` before falling back to system `gradle`.
- **Commands can timeout**: Default 5 min timeout. Large dependency trees on slow machines need more.
- **Scope normalization**: Unknown scopes default to `"compile"`. Valid: compile, runtime, test, provided, system.
- **`rootProject.name` vs `name`**: `rootProject.name` in `settings.gradle` is authoritative. `name` in `build.gradle` may be a task-internal name.
- **Composite builds**: `includeBuild` directives supported but deliberately excluded from module parsing.
- **CI detection**: `CI=true` or `JFROG_GRADLE_NO_DAEMON=true` → add `--console=plain --warning-mode=none --no-daemon`.

## Maven
- **No lock file**: Resolves dependencies at runtime every time. `mvn dependency:tree` is the only source.
- **HTTP 500 for 403 errors**: Maven can return 500 for what is actually 403 Forbidden. Check both status codes.
- **Test deps not in cache**: Test-scoped deps may not be in `~/.m2/repository/` unless `mvn test` was run.
- **Color output**: Add `-Dstyle.color=always` to preserve or `-B` for batch mode.
- **Classifier handling**: Artifacts may have classifiers: `{artifact}-{version}-{classifier}.{type}` vs `{artifact}-{version}.{type}`.
- **Maven Wrapper**: Check for `mvnw`/`mvnw.cmd` before system `mvn`.

## Docker
- **Checksums MUST come from server**: NEVER compute Docker layer checksums locally. AQL + Docker API headers only.
- **Remote repo caching**: Artifactory caches remote repo artifacts with `-cache` suffix. Search `{repo}-cache` not `{repo}`.
- **`_uploads` exclusion**: AQL must exclude `imageName/_uploads/*` (temporary staging).
- **Layer deduplication critical**: Duplicate layers break Artifactory promotion. Always deduplicate by SHA256.
- **Virtual repos without default deployment repo**: Cannot filter layers → returns empty results.
- **Marker layers**: Only in remote repos. `.marker` suffix files require Docker API download for real checksums.
- **Image name parsing complexity**: References arrive as `registry:port/repo/image:tag`, `image:tag`, `image@sha256:xxx`, etc.
