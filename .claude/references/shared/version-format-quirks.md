# Version Format Quirks by Package Manager

## Version Prefix Rules

| PM | Cache Filename Version Format | Has `v` Prefix | Example |
|---|---|---|---|
| **Cargo** | No prefix | No | `clap-4.5.41.crate` |
| **Poetry** | No prefix | No | `requests-2.31.0-py3-none-any.whl` |
| **Helm** | No prefix | No | `postgresql-14.3.3.tgz` |
| **Maven** | No prefix | No | `guava-31.1-jre.jar` |
| **Gradle** | No prefix | No | `guava-31.1-jre.jar` |
| **Go** | Has `v` prefix | Yes | `v1.2.3.zip` |
| **RubyGems** | No prefix | No | `rails-7.0.8.gem` |
| **npm** | No prefix in tarball | No | `express-4.18.2.tgz` |
| **Conan** | No prefix (uses `/` separator) | No | `zlib/1.2.13` |

## Multi-Candidate Search Pattern

When looking for cached package files, always try multiple version formats:

```go
func versionCandidates(version string) []string {
    return []string{
        strings.TrimPrefix(version, "v"), // "4.5.41" from "v4.5.41"
        version,                          // "v4.5.41" as-is
        "v" + strings.TrimPrefix(version, "v"), // Ensure "v" prefix
    }
}
```

**Real-world impact**: Before implementing multi-candidate search: 0/50 cache hits. After: 49/50 cache hits.

## Special Version Formats

| PM | Quirk | Example |
|---|---|---|
| **Maven** | SNAPSHOT suffix changes deployment repo | `1.0-SNAPSHOT` → snapshot repo |
| **Conan** | `name/version@user/channel` (1.x) vs `name/version` (2.x) | `zlib/1.2.13` |
| **Go** | Module path includes major version | `github.com/foo/bar/v2` |
| **Gradle** | Version conflict markers in tree output: `→` or `->` | `1.0 -> 1.1` means resolved to 1.1 |
| **pnpm** | `link:` prefix means workspace symlink, not a real version | `link:../shared-lib` — skip these |
| **Docker** | Image tags are not semantic versions | `latest`, `alpine`, `3.19-slim` |

## Normalization Rules

1. **Always strip `v` prefix** when constructing cache file paths (except Go)
2. **Always try both** with and without `v` when searching
3. **Never assume** the version format from the lock file matches the cache filename
4. **Cargo registry hash**: Cache subdirectory contains a registry hash (`index.crates.io-1949cf8c6b5b557f`) that varies per machine
5. **Gradle cache hash**: Subdirectory contains a SHA1 hash of the artifact — must walk the tree
