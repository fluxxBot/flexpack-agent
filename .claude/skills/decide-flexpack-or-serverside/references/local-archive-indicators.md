# Local Archive Indicators (→ FlexPack)

If ANY of these are found in the PM's local cache, the PM should use **FlexPack** architecture.

## File Type Indicators
- `.tgz` or `.tar.gz` — compressed tarballs (Helm charts, npm packages)
- `.whl` — Python wheel files (Poetry/pip)
- `.jar`, `.war`, `.aar` — Java archives (Maven, Gradle)
- `.crate` — Rust crate archives (Cargo)
- `.gem` — Ruby gem archives (RubyGems)
- `.zip` — Go module archives, generic packages
- `.tar.bz2`, `.tar.xz` — alternative compression formats

## Naming Pattern Indicators
- Files named `{package}-{version}.{ext}` (e.g., `clap-4.5.41.crate`)
- Files in a directory named after the package (e.g., `~/.m2/repository/com/google/guava/guava/31.1-jre/guava-31.1-jre.jar`)
- Files with version numbers in the filename

## Behavior Indicators
- Running `sha256sum <cached-file>` produces a checksum that matches the registry's published checksum
- The PM downloads complete artifacts before extracting/installing
- Cache size grows proportionally with number of dependencies
- Clearing the cache and re-installing re-downloads the same files

## Current FlexPack PMs
| PM | Cache File Type | Cache Location Pattern |
|---|---|---|
| Cargo | `.crate` | `~/.cargo/registry/cache/{hash}/` |
| Poetry | `.whl`, `.tar.gz` | `~/Library/Caches/pypoetry/artifacts/` (macOS) |
| Helm | `.tgz` | `~/Library/Caches/helm/repository/` (macOS) |
| Maven | `.jar`, `.war`, `.pom` | `~/.m2/repository/{groupPath}/` |
| Gradle | `.jar`, `.aar` | `~/.gradle/caches/modules-2/files-2.1/` |
| Conan | `.tgz`, `.tar.gz` | `~/.conan2/p/{hash}/` (use `conan cache path`) |
