---
name: generate-publish-buildinfo-collector
description: "Generates Go code for collecting build-info during package publish/upload. Tracks uploaded artifacts, tags build properties, and merges with install-side deps."
version: 1.0.0
user-invocable: false
---

# Generate Publish Build Info Collector

## Input
- PM name, upload protocol, artifact format

## Procedure
1. Load `.claude/references/templates/publish-buildinfo-template.md`
2. Generate artifact tracking (checksums on built artifacts, repo path construction)
3. Generate build property tagging from `.claude/references/templates/artifact-property-tagging.md`
4. Generate merge logic (publish artifacts + install dependencies → single BuildInfo)

## Output
Go code: `collectPublishBuildInfo()`, `tagBuildProperties()`
