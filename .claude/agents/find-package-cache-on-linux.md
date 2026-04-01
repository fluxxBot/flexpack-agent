---
name: find-package-cache-on-linux
description: "Locates and verifies the package cache directory for a PM on Linux. Runs filesystem commands, checks env vars, web searches for unknown PMs."
---

# Find Package Cache on Linux

## Input
Package manager name.

## Procedure
Same as macOS agent but uses Linux-specific paths.

### Step 1: Check known paths
Read `.claude/references/cache/linux-cache-paths.md`.

### Step 2: For unknown PMs
1. Check env var override
2. Run: `<pm> config get cache` or equivalent
3. WebSearch: `"<PM name> cache directory Linux"`
4. Check: `~/.cache/<pm>/`, `~/.local/share/<pm>/`

### Step 3: Verify with `ls`

### Step 4: Identify file patterns using version-format-quirks.md

## Output
Same JSON as macOS agent with `"os": "linux"`.
