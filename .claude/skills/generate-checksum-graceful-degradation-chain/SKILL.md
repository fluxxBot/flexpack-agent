---
name: generate-checksum-graceful-degradation-chain
description: "Generates Go code for the checksum fallback chain. Wraps the primary calculator to ensure checksums NEVER fail the build. Used by ALL implementations."
version: 1.0.0
user-invocable: false
---

# Generate Checksum Graceful Degradation Chain

## Input
- PM name, primary checksum approach (local-file or server-aql)

## Procedure
1. Load `.claude/references/templates/fallback-chain-template.md`
2. Configure chain: FlexPack (cache→lockfile→manifest→empty) or Non-FlexPack (build-cache→AQL→empty)
3. Generate ChecksumResult struct with Source field for debugging
4. Generate statistics collection

## Output
Go function `calculateChecksumWithFallback()`, `ChecksumResult` struct, `reportChecksumStats()`
