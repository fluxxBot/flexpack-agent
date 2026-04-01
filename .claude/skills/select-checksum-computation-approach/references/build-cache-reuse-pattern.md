# Build Cache Reuse Pattern (Tier 1)

## How It Works
Before querying AQL, check if a previous build with the same build name has published build-info. If yes, load its dependency checksums and reuse them. This avoids AQL entirely for repeat builds.

## Implementation
```go
func loadChecksumCache(sm artifactory.ArtifactoryServicesManager, buildName, projectKey string) (map[string]entities.Checksum, error) {
    previousBuildDeps, err := utils.GetDependenciesFromLatestBuild(sm, buildName, projectKey)
    if err != nil {
        log.Debug("No previous build found for cache:", err)
        return make(map[string]entities.Checksum), nil
    }
    return utils.DependenciesToChecksumMap(previousBuildDeps), nil
}
```

## Important Notes
- `DependenciesToChecksumMap` filters out deps where `Checksum.IsEmpty()` (all three fields empty)
- If the first build was published without checksums (e.g., resolved from public registry instead of Artifactory), the cache returns 0 hits even though the build exists
- **Implication**: The first build MUST resolve through Artifactory to populate checksums via AQL. Subsequent builds benefit from cache.
- This is a single API call — much cheaper than N batched AQL queries

## Usage Pattern
```go
cached, _ := loadChecksumCache(sm, buildName, projectKey)
var uncached []parsedDep
for _, d := range deps {
    key := d.name + ":" + d.version
    if cs, ok := cached[key]; ok && !cs.IsEmpty() {
        checksumMap[key] = cs  // Cache hit
    } else {
        uncached = append(uncached, d)  // Needs AQL
    }
}
// Only run AQL for uncached deps
```

## Applies To
- **Non-FlexPack only**: pnpm, Docker/Buildx
- FlexPack PMs compute checksums locally — no need for build cache
