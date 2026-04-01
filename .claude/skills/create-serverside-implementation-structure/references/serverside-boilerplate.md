# Non-FlexPack Boilerplate

```go
package {pm}

import (
    "fmt"
    "sync"
    buildinfo "github.com/jfrog/build-info-go/entities"
    "github.com/jfrog/jfrog-client-go/artifactory"
    "github.com/jfrog/jfrog-client-go/utils/log"
)

// --- install.go ---
type InstallCommand struct {
    serverDetails  *config.ServerDetails
    buildName, buildNumber, projectKey, workingDir string
    args []string
}

func (cmd *InstallCommand) Run() error {
    if err := cmd.runNativeInstall(); err != nil { return err }
    modules, err := cmd.collectDependencies()
    if err != nil {
        log.Warn("Build info collection failed:", err)
        return nil // Don't fail install
    }
    if err := cmd.resolveChecksums(modules); err != nil {
        log.Warn("Checksum resolution failed:", err)
    }
    return cmd.saveBuildInfo(modules)
}

// --- depresolver.go ---
type parsedDep struct {
    name, version string
    scopes        []string
    resolved      string // tarball URL
    repo          string // Artifactory repo
    dirPath       string // AQL path
    fileName      string // AQL filename
}

// --- checksums.go ---
func fetchChecksums(deps []parsedDep, sm artifactory.ArtifactoryServicesManager, buildName, projectKey string) (map[string]buildinfo.Checksum, error) {
    // Tier 1: Build cache
    cached, _ := loadChecksumCache(sm, buildName, projectKey)
    checksumMap := make(map[string]buildinfo.Checksum)
    var uncached []parsedDep
    for _, d := range deps {
        key := d.name + ":" + d.version
        if cs, ok := cached[key]; ok { checksumMap[key] = cs } else { uncached = append(uncached, d) }
    }
    // Tier 2: Batched AQL
    if len(uncached) > 0 {
        for k, v := range batchedAQLFetch(uncached, sm) { checksumMap[k] = v }
    }
    return checksumMap, nil
}

// --- registry.go ---
func extractRepoFromRegistryURL(registryURL string) string { return "" }
func resolveAqlRepo(repo string, sm artifactory.ArtifactoryServicesManager) string { return repo }
```
