# Build Cache Tier Template (Tier 1)

```go
func loadChecksumCache(sm artifactory.ArtifactoryServicesManager, buildName, projectKey string) (map[string]entities.Checksum, error) {
    previousBuildDeps, err := utils.GetDependenciesFromLatestBuild(sm, buildName, projectKey)
    if err != nil {
        log.Debug("No previous build cache:", err)
        return make(map[string]entities.Checksum), nil
    }
    return utils.DependenciesToChecksumMap(previousBuildDeps), nil
}

func splitByCacheHit(deps []parsedDep, cache map[string]entities.Checksum) (cached, uncached []parsedDep) {
    for _, d := range deps {
        key := d.name + ":" + d.version
        if cs, ok := cache[key]; ok && !cs.IsEmpty() {
            cached = append(cached, d)
        } else {
            uncached = append(uncached, d)
        }
    }
    return
}
```

**Note**: First build pays full AQL cost. Subsequent builds are fast (cache hit).
`DependenciesToChecksumMap` filters out deps where `IsEmpty()` is true.
