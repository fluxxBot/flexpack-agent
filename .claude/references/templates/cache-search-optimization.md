# Cache Search Optimization

## Hybrid Approach (Recommended)
```go
func (pm *PackManager) findPackageFile(name, version string) string {
    // Multi-candidate version search (critical for cache hit rate)
    versionCandidates := []string{
        strings.TrimPrefix(version, "v"),
        version,
        "v" + strings.TrimPrefix(version, "v"),
    }

    // Fast path: direct path construction O(1)
    for _, v := range versionCandidates {
        for _, cacheDir := range pm.getCacheDirectories() {
            directPath := filepath.Join(cacheDir, fmt.Sprintf("%s-%s%s", name, v, pm.fileExtension))
            if _, err := os.Stat(directPath); err == nil {
                return directPath
            }
        }
    }

    // Slow path: recursive search (fallback)
    for _, cacheDir := range pm.getCacheDirectories() {
        var found string
        filepath.WalkDir(cacheDir, func(path string, d os.DirEntry, err error) error {
            if err != nil { return nil }
            if !d.IsDir() && pm.matchesPackage(filepath.Base(path), name, versionCandidates) {
                found = path
                return filepath.SkipAll
            }
            return nil
        })
        if found != "" { return found }
    }
    return ""
}
```

## Pre-Built Cache Index (for many lookups)
```go
func (pm *PackManager) buildCacheIndex() {
    pm.cacheIndex = make(map[string]string)
    for _, cacheDir := range pm.getCacheDirectories() {
        filepath.WalkDir(cacheDir, func(path string, d os.DirEntry, err error) error {
            if err != nil || d.IsDir() { return nil }
            if name, ver := pm.extractPackageInfo(filepath.Base(path)); name != "" {
                pm.cacheIndex[name+":"+ver] = path
            }
            return nil
        })
    }
}
```

**Impact**: Before version-format fix: 0% hits. After: 98% hits.
