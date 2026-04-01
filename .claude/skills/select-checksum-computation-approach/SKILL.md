---
name: select-checksum-computation-approach
description: "Selects the checksum computation approach based on FlexPack (local files) or Non-FlexPack (server-side AQL). Determines the full priority chain, batch configuration, and fallback strategy."
version: 1.0.0
user-invocable: false
---

# Select Checksum Computation Approach

## Input
- Package manager name
- Architecture type (flexpack or non-flexpack) — from decide-flexpack-or-serverside

## Procedure

### Step 1: Read priority chain
Read `.claude/references/decision/checksum-source-priority-chain.md`.

### Step 2: Configure based on architecture

**If FlexPack (local files):**
- Priority: local cache files → lockfile checksums → manifest-based → empty
- Read cache file extension and pattern from cache location skill output
- Read `.claude/references/shared/version-format-quirks.md` for version handling

**If Non-FlexPack (server-side):**
- Priority: build-cache (Tier 1) → batched AQL (Tier 2) → empty
- Read `.claude/references/decision/aql-query-patterns.md` for query format
- Read `.claude/references/decision/build-cache-reuse-pattern.md` for Tier 1 caching
- Configure: batch_size=30, worker_count=15

### Step 3: Always ensure graceful degradation
The final fallback is ALWAYS empty checksums. Never fail the build.

## Output Format
```json
{
  "package_manager": "<name>",
  "approach": "local-files | server-aql",
  "priority_chain": ["cache-file", "lockfile-checksum", "manifest-fallback", "empty"],
  "file_extension": ".<ext> | N/A",
  "batch_config": { "batch_size": 30, "worker_count": 15 },
  "aql_includes": ["repo", "path", "name", "actual_sha1", "sha256", "actual_md5"],
  "build_cache_enabled": true,
  "fallback_to_empty": true
}
```
