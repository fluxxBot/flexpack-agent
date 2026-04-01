# Scope Inheritance Rules

```go
func (pm *PackManager) determineScopes(depName string) []string {
    var scopes []string
    if pm.isDirectDep(depName)    { scopes = append(scopes, "runtime") }
    if pm.isDevDep(depName)       { scopes = append(scopes, "dev") }
    if pm.isTestDep(depName)      { scopes = append(scopes, "test") }
    if pm.isBuildDep(depName)     { scopes = append(scopes, "build") }
    if len(scopes) == 0           { scopes = append(scopes, "transitive") }
    return scopes
}
```

## Propagation Rules
- compile parent → compile child (propagates)
- runtime parent → runtime child (propagates)
- test parent → test child (propagates, often filtered)
- dev parent → dev child (propagates)
- provided parent → does NOT propagate (Maven-specific)
- If dep appears in multiple scopes, keep ALL (e.g., `["runtime", "test"]`)
- If dep not direct and not reachable from direct → "transitive"
