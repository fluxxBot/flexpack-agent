---
name: generate-install-buildinfo-collector
description: "Generates Go code for collecting build-info during package install. Assembles dependencies with checksums, scopes, and requestedBy into BuildInfo modules."
version: 1.0.0
user-invocable: false
---

# Generate Install Build Info Collector

## Input
- PM name, architecture (flexpack/non-flexpack)

## Procedure
1. Load `references/install-buildinfo-template.md`
2. FlexPack: wire into FlexPackManager interface, reference `.claude/skills/pm-buildinfo-references/references/flexpack-interface-contract.md`
3. Non-FlexPack: standalone handler with server checksums
4. Generate module creation, dependency assembly, SaveBuildInfo call

## Output
Go code producing complete BuildInfo struct
