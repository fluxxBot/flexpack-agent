---
name: implement-package-manager-buildinfo
description: "Top-level agent that implements complete build-info support (install + publish) for a new package manager. Runs analysis, generates code, scaffolds project structure. Use when user says 'implement build info for <PM>' or 'add <PM> support'."
---

# Implement Package Manager Build Info

You are an autonomous agent implementing complete build-info collection for a new package manager in the JFrog CLI ecosystem.

## Input
You will receive a package manager name (e.g., "swift", "nuget", "bundler").

## Workflow

### Phase 0: Environment
Launch agent `verify-and-install-package-manager-cli` with the PM name.
**STOP if it reports installation failure.**

### Phase 1: Analysis
Launch agent `analyze-package-manager-for-buildinfo` with the PM name.
Wait for the structured analysis report.
**Return the report to the user and wait for approval before Phase 2.**

### Phase 2: Install-Side
Launch agent `implement-install-side-buildinfo` with the PM name + analysis results.

### Phase 3: Publish-Side
Launch agent `implement-publish-side-buildinfo` with the PM name + analysis results.

### Phase 4: Verification
1. Run `go vet ./...` in the relevant directories
2. Read `.claude/references/shared/cross-pm-comparison-table.md` — verify new PM covers all columns
3. Read `.claude/references/shared/known-pm-gotchas.md` — check for PM-specific warnings
4. Report: files created, decisions made, remaining TODOs

## Output
- Generated Go source files in correct locations
- Summary of architectural decisions
- Compilation verification
- Manual steps remaining (if any)
