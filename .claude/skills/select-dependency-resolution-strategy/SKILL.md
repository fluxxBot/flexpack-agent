---
name: select-dependency-resolution-strategy
description: "Selects the dependency resolution strategy (lockfile-direct, CLI-based, or hybrid) for a package manager. Determines which commands to run, what output to parse, and whether transitive dependencies are included."
version: 1.0.0
user-invocable: false
---

# Select Dependency Resolution Strategy

## Input
- Package manager name

## Procedure

### Step 1: Check lock file availability
Read `references/lockfile-formats-catalog.md`. Does the PM produce a lock file?

### Step 2: Check CLI tree command availability
Read `references/cli-resolution-commands.md`. Does the PM have a CLI tree command?

### Step 3: Apply decision matrix
- Has lock file + has CLI → **hybrid** (prefer CLI for rich data, lock file as fallback)
- Has lock file only → **lockfile-direct**
- Has CLI only (no lock file, e.g., Maven, Gradle) → **cli-based**
- Neither → WebSearch for `"<PM name> dependency resolution"`

### Step 4: Determine manifest format
Read `.claude/skills/pm-buildinfo-references/references/manifest-and-lockfile-field-maps.md` for struct definitions.

## Output Format
```json
{
  "package_manager": "<name>",
  "primary_strategy": "lockfile-direct | cli-based | hybrid",
  "cli_command": "<full command> | null",
  "cli_output_format": "json | text-tree | null",
  "lock_file_name": "<filename> | null",
  "lock_file_format": "toml | yaml | json | null",
  "manifest_file": "<filename>",
  "manifest_format": "toml | json | yaml | xml | line-based | dsl",
  "handles_transitives": true,
  "fallback_strategy": "<description>"
}
```
