---
name: generate-lockfile-direct-resolver
description: "Generates Go code for resolving dependencies by directly mapping lock file entries to DependencyInfo structs. Used when the lock file contains complete resolved versions."
version: 1.0.0
user-invocable: false
---

# Generate Lock File Direct Resolver

## Input
- PM name, lock file format, lock file struct definition

## Procedure
1. Load `.claude/references/templates/lockfile-resolver-template.md`
2. Map lock entries → DependencyInfo (ID, Name, Version, Type, Scopes)
3. Handle: deduplication, skip-self, optional/dev sections, malformed entries

## Output
Go function `parseDependenciesFromLockFile()` returning `[]DependencyInfo`
