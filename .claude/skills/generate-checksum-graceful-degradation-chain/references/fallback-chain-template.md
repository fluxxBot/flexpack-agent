# Checksum Graceful Degradation Template

```go
type ChecksumResult struct {
    SHA1   string
    SHA256 string
    MD5    string
    Source string // "cache-file", "lockfile", "manifest", "aql", "build-cache", "empty"
    Path   string
}

func (pm *PackManager) calculateChecksumWithFallback(dep utils.DependencyInfo) ChecksumResult {
    // Strategy 1: Primary (varies by architecture)
    if filePath := pm.findPackageFile(dep.Name, dep.Version); filePath != "" {
        if sha1, sha256, md5, err := pm.calculateFileChecksum(filePath); err == nil {
            return ChecksumResult{SHA1: sha1, SHA256: sha256, MD5: md5, Source: "cache-file", Path: filePath}
        }
    }

    // Strategy 2: Lock file checksums
    if cs := pm.lockFileChecksum(dep); cs != "" {
        return ChecksumResult{SHA256: cs, Source: "lockfile"}
    }

    // Strategy 3: Empty — NEVER fail the build
    log.Debug("No checksum for", dep.Name, dep.Version, "— continuing with empty")
    return ChecksumResult{Source: "empty"}
}

func (pm *PackManager) reportChecksumStats(results []ChecksumResult) {
    stats := make(map[string]int)
    for _, r := range results { stats[r.Source]++ }
    total := len(results)
    for src, count := range stats {
        log.Info(fmt.Sprintf("Checksum %s: %d/%d (%.1f%%)", src, count, total, float64(count)/float64(total)*100))
    }
}
```

**NON-NEGOTIABLE RULE**: Never fail the build due to missing checksums. Always fall back to empty.
