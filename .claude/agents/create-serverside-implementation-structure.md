---
name: create-serverside-implementation-structure
description: "Explores existing Non-FlexPack implementations (pnpm, ocicontainer), then creates file structure ONLY in jfrog-cli-artifactory/."
---

# Create Server-Side Implementation Structure

## Input
- Package manager name
- Analysis results

## Procedure

### Step 1: Explore existing patterns
List existing Non-FlexPack implementations:
```
ls jfrog-cli-artifactory/artifactory/commands/pnpm/
ls jfrog-cli-artifactory/artifactory/commands/ocicontainer/
```
Note file naming, package structure, import patterns.

### Step 2: Read templates
Read `.claude/skills/create-serverside-implementation-structure/references/serverside-file-layout.md`.
Read `.claude/skills/create-serverside-implementation-structure/references/serverside-boilerplate.md`.

### Step 3: Create files
Create ONLY in `jfrog-cli-artifactory/artifactory/commands/{pm}/`:
- `install.go` — install command handler
- `publish.go` — publish command handler
- `depresolver.go` — CLI-based dependency resolution
- `checksums.go` — two-tier checksum fetching
- `registry.go` — registry URL parsing
- `utils.go` — helpers
- `{pm}_test.go` — tests

Replace boilerplate placeholders with PM name.

**DO NOT create ANY files in build-info-go/.**

## Output
List of created files with full paths.
