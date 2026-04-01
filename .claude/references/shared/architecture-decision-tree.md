# FlexPack vs Non-FlexPack Architecture Decision Tree

## The Core Question

```
Does the package manager store original downloadable archives
(tarballs, JARs, wheels, etc.) in a local cache directory?
│
├─ YES → FlexPack architecture
│   ├─ Code in: build-info-go/flexpack/ + jfrog-cli-artifactory/.../flexpack/
│   ├─ Checksums: computed locally from cached files via crypto.GetFileDetails()
│   └─ Examples: Poetry (.whl), Cargo (.crate), Maven (.jar), Helm (.tgz), Conan (archives), Gradle (.jar)
│
└─ NO → Non-FlexPack (Server-Side) architecture
    ├─ Code in: jfrog-cli-artifactory/.../commands/<pkg-manager>/ ONLY
    ├─ Checksums: fetched from Artifactory via AQL or API
    ├─ No code in build-info-go/ at all
    └─ Examples: pnpm (content-addressable store), Docker (layers in registry)
```

## How to Verify

### Signs of FlexPack (local archives exist)
- Cache directory contains files matching `{package}-{version}.{ext}` pattern
- File extensions: `.tgz`, `.tar.gz`, `.whl`, `.jar`, `.crate`, `.gem`, `.zip`, `.aar`
- Files can be checksummed and the checksum matches what Artifactory stores
- Running `ls <cache-dir>` shows human-readable filenames

### Signs of Non-FlexPack (content-addressable / no local archives)
- Cache uses content-addressable storage (files named by hash, not by package name)
- Hard links or symlinks to a shared store
- No original archive files preserved after extraction
- Virtual filesystem or FUSE mount
- Package content is unpacked directly, original tarball discarded

## Code Location Implications

| Architecture | Core Logic | CLI Integration | Tests |
|---|---|---|---|
| **FlexPack** | `build-info-go/flexpack/{pm}_flexpack.go` | `jfrog-cli-artifactory/artifactory/commands/flexpack/{pm}.go` | `build-info-go/flexpack/tests/unit/{pm}_flexpack_test.go` |
| **Non-FlexPack** | N/A — no code in build-info-go | `jfrog-cli-artifactory/artifactory/commands/{pm}/*.go` | `jfrog-cli-artifactory/artifactory/commands/{pm}/{pm}_test.go` |

## Verification Procedure for Unknown PMs

1. Install the PM and a sample project
2. Run the install/fetch command to populate the cache
3. Find the cache directory (`<pm> config get cache` or WebSearch)
4. `ls` the cache — look for archive files
5. If archives found → FlexPack. If hash-named files or empty → Non-FlexPack.
