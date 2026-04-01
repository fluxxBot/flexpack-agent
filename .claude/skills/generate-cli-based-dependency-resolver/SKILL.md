---
name: generate-cli-based-dependency-resolver
description: "Generates Go code for resolving dependencies by executing a CLI command (cargo tree, pnpm ls --json, mvn dependency:tree). Includes command execution, output parsing, and transitive dep discovery."
version: 1.0.0
user-invocable: false
---

# Generate CLI-Based Dependency Resolver

## Input
- PM name, CLI command, output format (json/text-tree)

## Procedure
1. Load `references/cli-resolver-template.md`
2. If JSON → direct unmarshal, recursive walk with dedup
3. If text tree → indentation parser from `references/cli-output-parsing-patterns.md`
4. Generate: `resolveWithCLI()`, `parseCliOutput()`, timeout handling, scope extraction

## Output
Go functions for CLI-based dependency resolution
