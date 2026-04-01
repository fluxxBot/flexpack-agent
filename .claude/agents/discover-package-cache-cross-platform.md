---
name: discover-package-cache-cross-platform
description: "Discovers package cache directories across all 3 OSes. Launches OS-specific cache agents in parallel, produces unified result with Go code snippet."
---

# Discover Package Cache Cross-Platform

## Input
Package manager name.

## Procedure

### Step 1: Determine current OS
Run `uname -s` to identify the current platform.

### Step 2: Launch OS-specific agents
Launch the matching OS agent for **verified** paths on current OS:
- macOS → `find-package-cache-on-macos`
- Linux → `find-package-cache-on-linux`

For the other two OSes, read their reference files directly (can't verify remotely):
- `.claude/skills/find-package-cache-on-macos/references/macos-cache-paths.md`
- `.claude/skills/find-package-cache-on-linux/references/linux-cache-paths.md`
- `.claude/skills/find-package-cache-on-windows/references/windows-cache-paths.md`

### Step 3: Get env var override
Read `.claude/skills/discover-package-cache-cross-platform/references/env-var-overrides.md`.

### Step 4: Generate Go code
Produce the `getCacheDirectories()` function with `runtime.GOOS` switch and env var priority.

## Output
```json
{
  "package_manager": "<name>",
  "env_var_override": "<VAR>",
  "current_os": "darwin | linux | windows",
  "paths": { "darwin": [], "linux": [], "windows": [] },
  "current_os_verified": true,
  "file_pattern": "<pattern>",
  "go_code_snippet": "func getCacheDirectories() []string { ... }"
}
```
