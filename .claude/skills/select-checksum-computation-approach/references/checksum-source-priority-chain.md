# Checksum Source Priority Chain (Non-Negotiable)

```
Priority 1: Local cache files (tarballs, jars, wheels, crates, gems)
    ↓ not found
Priority 2: Local lockfile checksums (e.g., Cargo.lock checksum field, go.sum)
    ↓ not available
Priority 3: Previous build-info cache from Artifactory (GetDependenciesFromLatestBuild)
    ↓ not found or no checksums
Priority 4: Server-side AQL queries against Artifactory repos
    ↓ no results
Priority 5: Manifest-based deterministic checksums (last resort)
    ↓ all failed
Priority 6: Empty checksums — NEVER fail the build
```

## FlexPack Priority Chain
1. **Local cache files** — `crypto.GetFileDetails(filePath, true)` → SHA1, SHA256, MD5
2. **Lockfile checksums** — e.g., Cargo.lock `checksum` field, go.sum hashes
3. **Manifest-based** — deterministic hash of `name:version:type` metadata
4. **Empty** — continue with empty strings, log warning

## Non-FlexPack Priority Chain
1. **Build cache** — `GetDependenciesFromLatestBuild(sm, buildName, projectKey)` → single API call
2. **Batched AQL** — `items.find({"repo":"<repo>","$or":[...]}).include(...)` → batch of 30, 15 workers
3. **Empty** — continue with empty strings, log warning

## Per-PM Checksum Targets

| PM | File Extension | Checksum Target | Notes |
|---|---|---|---|
| Cargo | `.crate` | Archive file | No `v` prefix in filename |
| Poetry | `.whl`, `.tar.gz` | Archive file | Both wheel and sdist |
| Helm | `.tgz` | Archive file | From helm repo cache |
| Maven | `.jar`, `.war`, `.pom` | Archive file | GroupId→path conversion |
| Gradle | `.jar`, `.aar` | Archive file | Hash subdirectory in cache |
| Conan | `.tgz`, `.tar.gz` | Archive in cache | Use `conan cache path` to locate |
| pnpm | N/A | **AQL from server** | Content-addressable store, no tarballs |
| Docker | N/A | **AQL + Docker API** | Layers on registry only |
