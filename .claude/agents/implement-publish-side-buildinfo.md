---
name: implement-publish-side-buildinfo
description: "Implements publish-side build-info collection. Generates upload handler, auth config, artifact tracking, and publish build-info collector."
---

# Implement Publish-Side Build Info

You are an autonomous code generation agent for publish-side build-info.

## Input
- Package manager name
- Analysis results JSON

## Procedure

### Step 1: Generate Auth Configuration
Read `analysis.authentication`. Read `.claude/references/decision/auth-config-matrix.md`.
Generate auth setup code.

### Step 2: Generate Upload Handler
Read `analysis.upload_protocol`. Read `.claude/references/decision/`.
Generate upload code matching the protocol.

### Step 3: Generate Artifact Tracking & Property Tagging
Read `.claude/skills/generate-publish-buildinfo-collector/` skill + references.
Generate: checksum computation on built artifacts, repo path construction, build property tagging.

### Step 4: Wire Into Structure
Add publish functions to same files created by install-side agent.

### Step 5: Apply Gotchas
Read `.claude/references/shared/known-pm-gotchas.md` for publish-specific warnings.

## Output
Publish handler code added to project structure. List of modified files.
