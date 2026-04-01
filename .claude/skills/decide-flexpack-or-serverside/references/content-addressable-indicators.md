# Content-Addressable Store Indicators (→ Non-FlexPack)

If these are found instead of named archives, the PM should use **Non-FlexPack** (Server-Side) architecture.

## Store Structure Indicators
- Files named by content hash (e.g., `sha256-abc123def456...` or `integrity-sha512-...`)
- Flat or sharded hash directories (e.g., `ab/cd/ef/abcdef123456/`)
- Hard links from `node_modules/` (or equivalent) to a shared store
- Symlinks pointing to a central content-addressed location

## Behavior Indicators
- No `.tgz`, `.whl`, `.jar`, or other recognizable archive files in cache
- Original tarballs are downloaded, extracted, and then DELETED
- Cache stores individual files by content hash, not by package identity
- Multiple packages sharing the same file content point to the same hash entry
- The PM uses a "virtual" node_modules structure (e.g., pnpm's symlinked approach)

## Verification Steps
1. Run `find <cache-dir> -name "*.tgz" -o -name "*.tar.gz"` — if empty, likely content-addressable
2. Check if files are named with hash prefixes: `ls <cache-dir> | head -20`
3. Check for hard links: `find <cache-dir> -links +1 -type f | head -5`

## Current Non-FlexPack PMs
| PM | Cache Type | Why No Local Archives |
|---|---|---|
| pnpm | Content-addressable store (`~/.pnpm-store/`) | Files stored by content hash, hard-linked into projects. Original tarballs not preserved. |
| Docker | OCI registry layers | Image layers exist as blobs on the registry server. There are no local "package files" to checksum. |
| Yarn (Berry/v2+) | Content-addressable (similar to pnpm) | `.yarn/cache/` exists but uses zip archives with integrity checks — may be FlexPack candidate depending on version. Verify. |

## Important Note
When Non-FlexPack, checksums come from Artifactory server:
- **Tier 1**: Previous build-info cache (`GetDependenciesFromLatestBuild()`) — single API call
- **Tier 2**: Batched AQL queries — `items.find({"repo":"<repo>",...}).include("actual_sha1","sha256","actual_md5")`
