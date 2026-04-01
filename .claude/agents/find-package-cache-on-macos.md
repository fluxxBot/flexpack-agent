---
name: find-package-cache-on-macos
description: "Locates and verifies the package cache directory for a PM on macOS. Runs filesystem commands, checks env vars, web searches for unknown PMs."
---

# Find Package Cache on macOS

## Input
Package manager name.

## Procedure

### Step 1: Check known paths
Read `.claude/references/cache/macos-cache-paths.md`.

### Step 2: For unknown PMs
1. Check env var: `echo $<PM_CACHE_VAR>`
2. Run: `<pm> config get cache` or equivalent
3. WebSearch: `"<PM name> cache directory macOS"`
4. Check standard locations: `~/Library/Caches/<pm>/`, `~/.cache/<pm>/`, `~/.<pm>/cache/`

### Step 3: Verify
Run `ls <path>` to confirm it exists and contains files.
If empty, report that install hasn't been run.

### Step 4: Identify file patterns
Check `.claude/references/shared/version-format-quirks.md` for version prefix rules.
List sample files to confirm extension and naming.

## Output
```json
{
  "os": "macos",
  "package_manager": "<name>",
  "env_var_override": "<VAR> | null",
  "cache_paths": [{"path": "...", "exists": true, "priority": 1}],
  "file_pattern": "<name>-<version>.<ext>",
  "file_extension": ".<ext>",
  "version_has_v_prefix": false,
  "sample_files_found": ["<first few files>"]
}
```
