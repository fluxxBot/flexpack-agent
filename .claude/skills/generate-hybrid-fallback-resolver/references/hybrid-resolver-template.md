# Hybrid Resolver Template (Validated by Poetry & Cargo)

```go
func (pm *PackManager) resolveDependencies() {
    // Strategy 1: Prefer lock file + manifest if both available
    if pm.lockFileData != nil && pm.manifestData != nil {
        pm.parseFromFiles()
        if len(pm.dependencies) > 0 {
            log.Debug("Resolved", len(pm.dependencies), "dependencies from lock file")
            return
        }
        log.Debug("Lock file parsing yielded no dependencies, trying CLI")
    }

    // Strategy 2: CLI-based resolution (most accurate when tool is installed)
    if err := pm.resolveWithCLI(); err == nil {
        log.Debug("Resolved", len(pm.dependencies), "dependencies via CLI")
        return
    } else {
        log.Debug("CLI resolution failed:", err, "— falling back to file parsing")
    }

    // Strategy 3: Final fallback to file-only parsing
    pm.parseFromFiles()
    log.Debug("Resolved", len(pm.dependencies), "dependencies from files (fallback)")
}
```

## Why This Order Works
- **Lock file first**: Deterministic, no CLI dependency, fast
- **CLI second**: Gives richer data (transitives, resolved versions) when available
- **File-only last**: Always available, but may miss transitives

## Merge Logic (when both manifest and lock exist)
```go
func (pm *PackManager) parseFromFiles() {
    // Lock file provides: resolved versions, transitive deps
    // Manifest provides: scope classification (direct vs dev)
    directDeps := pm.parseManifestDeps()
    for _, pkg := range pm.lockFileData.Packages {
        scopes := []string{"transitive"}
        if directDeps[pkg.Name] { scopes = []string{"runtime"} }
        if devDeps[pkg.Name] { scopes = []string{"dev"} }
        // ... build DependencyInfo
    }
}
```
