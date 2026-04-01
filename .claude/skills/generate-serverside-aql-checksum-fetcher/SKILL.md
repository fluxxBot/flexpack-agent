---
name: generate-serverside-aql-checksum-fetcher
description: "Generates Go code for fetching checksums from Artifactory via batched AQL queries with build-cache tier. Used by Non-FlexPack PMs (pnpm, Docker) that don't store local archives."
version: 1.0.0
user-invocable: false
---

# Generate Server-Side AQL Checksum Fetcher

## Input
- PM name, tarball URL format, registry URL pattern

## Procedure
1. Generate Tier 1 (build cache) from `references/build-cache-tier-template.md`
2. Generate Tier 2 (batched AQL) from `references/aql-batch-query-template.md`
3. Generate concurrent worker pool from `references/concurrent-worker-pool-template.md`
4. Generate registry URL parser and repo resolver
5. Wire into main `fetchChecksums()` function

## Output
Go files: `checksums.go` (two-tier fetch), `registry.go` (URL parsing, repo resolution)
