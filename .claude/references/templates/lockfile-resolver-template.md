# Lock File Direct Resolver Template

```go
func (pm *PackManager) parseDependenciesFromLockFile() error {
    if pm.lockFileData == nil {
        return fmt.Errorf("lock file not loaded")
    }

    seen := make(map[string]bool)
    for _, pkg := range pm.lockFileData.Packages {
        // Skip the project itself
        if pkg.Name == pm.projectName {
            continue
        }

        depID := fmt.Sprintf("%s:%s", pkg.Name, pkg.Version)

        // Deduplicate
        if seen[depID] {
            continue
        }
        seen[depID] = true

        dep := utils.DependencyInfo{
            Type:    pm.packageType,
            ID:      depID,
            Name:    pkg.Name,
            Version: pkg.Version,
            Scopes:  pm.determineScopes(pkg.Name),
        }

        pm.dependencies = append(pm.dependencies, dep)
    }

    log.Debug(fmt.Sprintf("Parsed %d dependencies from lock file", len(pm.dependencies)))
    return nil
}
```

## Error Handling
- Skip entries with empty name or version (log warning)
- Continue on malformed entries — don't fail the entire parse
- Log total count for debugging
