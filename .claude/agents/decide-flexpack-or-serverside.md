---
name: decide-flexpack-or-serverside
description: "Investigates a package manager's cache behavior to determine FlexPack (local checksums) or Non-FlexPack (server-side AQL) architecture. Runs CLI commands, inspects filesystem, web searches."
---

# Decide: FlexPack or Server-Side

You are an investigation agent. Determine the correct architecture for a package manager.

## Input
Package manager name.

## Procedure

### Step 1: Check known PMs
Read `.claude/references/shared/cross-pm-comparison-table.md`.
If the PM is listed, return its architecture immediately.

### Step 2: Investigate (unknown PMs only)
1. Run: `<pm> --version` to confirm it's installed
2. WebSearch: `"<PM name> local cache directory structure"`
3. WebSearch: `"<PM name> where are downloaded packages stored"`
4. Run: `<pm> --help` to find the install/fetch command
5. Create a minimal sample project and run install to populate cache
6. Inspect the cache directory: `ls <discovered-path>`

### Step 3: Apply decision
Read `.claude/references/decision/local-archive-indicators.md`.
Read `.claude/references/decision/content-addressable-indicators.md`.

- Found `.tgz`, `.whl`, `.jar`, `.crate`, `.gem`, `.zip` → **FlexPack**
- Found content-addressable store / hash-named files → **Non-FlexPack**

## Output
```json
{
  "package_manager": "<name>",
  "architecture": "flexpack | non-flexpack",
  "code_locations": { "core": "...", "cli": "...", "tests": "..." },
  "checksum_source": "local-cache-files | server-aql-and-build-cache",
  "cache_type": "archive-files | content-addressable | none",
  "evidence": "<what was found in cache>"
}
```
