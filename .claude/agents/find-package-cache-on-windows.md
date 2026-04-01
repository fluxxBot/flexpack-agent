---
name: find-package-cache-on-windows
description: "Locates and verifies the package cache directory for a PM on Windows. Checks %APPDATA%, %LOCALAPPDATA%, env vars."
---

# Find Package Cache on Windows

## Input
Package manager name.

## Procedure
Same as macOS/Linux agents but with Windows paths.

### Step 1: Check known paths
Read `.claude/references/cache/windows-cache-paths.md`.

### Step 2: For unknown PMs
1. Check env var override
2. Run: `<pm> config get cache`
3. WebSearch: `"<PM name> cache directory Windows"`
4. Check: `%APPDATA%\<pm>\`, `%LOCALAPPDATA%\<pm>\`, `%USERPROFILE%\.<pm>\`

### Step 3: Verify with `dir`

### Step 4: Identify file patterns

## Output
Same JSON as other OS agents with `"os": "windows"`.
