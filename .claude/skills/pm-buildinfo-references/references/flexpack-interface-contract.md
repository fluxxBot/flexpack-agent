# FlexPack Interface Contract

## Core Interface

```go
type FlexPackManager interface {
    GetDependency() string                           // Human-readable dependency summary
    ParseDependencyToList() []string                 // List of dependency IDs ("name:version")
    CalculateChecksum() []map[string]interface{}     // Checksums for all dependencies
    CalculateScopes() []string                       // Available scope types
    CalculateRequestedBy() map[string][]string       // Inverse dependency graph
}
```

## Configuration Structure

```go
type PackageManagerConfig struct {
    WorkingDirectory string            // Project root directory
    BuildName        string            // Build name for build-info
    BuildNumber      string            // Build number for build-info
    ProjectKey       string            // JFrog project key (optional)
    ServerDetails    *config.ServerDetails // Artifactory connection (optional for FlexPack)
    Args             []string          // Additional CLI arguments
}
```

## Dependency Info Structure

```go
type DependencyInfo struct {
    Type    string   // Package type ("poetry", "cargo", "maven", etc.)
    ID      string   // Unique identifier "name:version"
    Name    string   // Package name
    Version string   // Resolved version
    Scopes  []string // Dependency scopes ["runtime", "dev", "test", etc.]
}
```

## State Management Patterns

### Lazy Loading (Recommended for most PMs)
```go
func (pm *MyFlexPack) ensureDependenciesLoaded() {
    if len(pm.dependencies) == 0 {
        pm.resolveDependencies()
    }
}
```

### Eager Loading (For PMs where parsing is cheap)
```go
func NewMyFlexPack(config PackageManagerConfig) (*MyFlexPack, error) {
    pm := &MyFlexPack{config: config}
    if err := pm.loadAllData(); err != nil {
        return nil, err
    }
    return pm, nil
}
```

## Constructor Pattern

```go
func New{Pm}FlexPack(config utils.PackageManagerConfig) (*{Pm}FlexPack, error) {
    pm := &{Pm}FlexPack{
        config:          config,
        dependencyGraph: make(map[string][]string),
    }
    if err := pm.loadManifest(); err != nil {
        return nil, fmt.Errorf("failed to load manifest: %w", err)
    }
    if err := pm.loadLockFile(); err != nil {
        log.Printf("Warning: lock file not found: %v", err)
        // Lock file is optional for some PMs
    }
    return pm, nil
}
```
