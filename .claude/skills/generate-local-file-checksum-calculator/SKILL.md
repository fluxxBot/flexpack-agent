---
name: generate-local-file-checksum-calculator
description: "Generates Go code for computing checksums from locally cached package files (tarballs, JARs, wheels, crates). Used by FlexPack PMs that store original archives in local cache."
version: 1.0.0
user-invocable: false
---

# Generate Local File Checksum Calculator

## Input
- PM name, cache paths, file pattern, file extension

## Procedure
1. Generate package file search from `references/cache-search-optimization.md` (hybrid: direct path O(1) then recursive walk)
2. Generate single-file checksum from `references/single-file-checksum-template.md` (crypto.GetFileDetails)
3. Apply version format handling from `.claude/skills/pm-buildinfo-references/references/version-format-quirks.md`
4. Wire into CalculateChecksum() interface method

## Output
Go functions: `findPackageFile()`, `calculateFileChecksum()`, `CalculateChecksum()`
