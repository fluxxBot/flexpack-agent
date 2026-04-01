---
name: classify-dependency-scopes
description: "Generates Go code for classifying dependency scopes (compile, runtime, test, dev, transitive). Maps PM-specific categories to standardized scope labels."
version: 1.0.0
user-invocable: false
---

# Classify Dependency Scopes

## Input
- Package manager name

## Procedure
1. Look up PM in `.claude/references/scopes/scope-type-catalog.md`
2. Select pattern: manifest-based (most PMs), context-based (Conan), configuration-based (Gradle/Maven)
3. Apply inheritance rules from `.claude/references/scopes/scope-inheritance-rules.md`
4. Generate `determineScopes()` and `CalculateScopes()` methods

## Output
Go scope classification functions
