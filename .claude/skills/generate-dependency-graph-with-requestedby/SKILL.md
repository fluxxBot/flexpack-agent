---
name: generate-dependency-graph-with-requestedby
description: "Generates Go code for constructing a dependency graph and computing the RequestedBy inverse mapping."
version: 1.0.0
user-invocable: false
---

# Generate Dependency Graph with RequestedBy

## Input
- PM name, resolution strategy

## Procedure
1. Select graph representation from `.claude/references/templates/graph-construction-patterns.md` (adjacency list default)
2. Select construction: top-down (from CLI tree output) or bottom-up (from lock file)
3. Generate RequestedBy inversion from `.claude/references/templates/graph-inversion-for-requestedby.md`
4. Add cycle detection and diamond dependency deduplication

## Output
Go code: `CalculateRequestedBy()` returning `map[string][]string`
