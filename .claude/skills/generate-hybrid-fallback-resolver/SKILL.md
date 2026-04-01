---
name: generate-hybrid-fallback-resolver
description: "Generates Go code for hybrid resolution: try CLI first, fall back to lock file parsing. Maximum reliability — CLI gives rich data, file parsing works without the tool installed."
version: 1.0.0
user-invocable: false
---

# Generate Hybrid Fallback Resolver

## Input
- PM name, CLI command, lock file struct

## Procedure
1. Load `references/hybrid-resolver-template.md`
2. Generate three-strategy pattern: lock+manifest → CLI → file-only fallback
3. Include logging at each strategy transition

## Output
Go function with try-CLI-then-fallback pattern
