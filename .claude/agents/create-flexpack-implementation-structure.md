---
name: create-flexpack-implementation-structure
description: "Explores the existing codebase for naming patterns, then creates the FlexPack file structure in build-info-go/flexpack/ AND jfrog-cli-artifactory/."
---

# Create FlexPack Implementation Structure

## Input
- Package manager name
- Analysis results

## Procedure

### Step 1: Explore existing patterns
List existing FlexPack implementations to match conventions:
```
ls build-info-go/flexpack/*.go
ls build-info-go/flexpack/*/
ls jfrog-cli-artifactory/artifactory/commands/flexpack/
```
Note the naming patterns (struct names, file names, package names).

### Step 2: Read templates
Read `.claude/skills/create-flexpack-implementation-structure/references/flexpack-file-layout.md` for structure.
Read `.claude/skills/create-flexpack-implementation-structure/references/flexpack-boilerplate.md` for Go boilerplate.
Read `.claude/skills/pm-buildinfo-references/references/flexpack-interface-contract.md` for the interface.

### Step 3: Determine simple vs complex
- Simple PM (like Poetry, Cargo, Helm): single file `{pm}_flexpack.go`
- Complex PM (like Conan, Gradle): subdirectory `{pm}/` with multiple files

### Step 4: Create files
Replace `{Pm}` placeholders in boilerplate with actual PM name (e.g., `Swift`, `Nuget`).
Create files in:
- `build-info-go/flexpack/` (core implementation)
- `jfrog-cli-artifactory/artifactory/commands/flexpack/` (CLI integration)
- `build-info-go/flexpack/tests/unit/` (tests)

**DO NOT create new top-level directories.**

## Output
List of created files with full paths.
