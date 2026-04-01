# Install Build Info Template

## FlexPack Pattern
```go
func (pm *PackManager) collectInstallBuildInfo() (*entities.BuildInfo, error) {
    pm.resolveDependencies()
    var csResults []ChecksumResult
    for _, dep := range pm.dependencies {
        csResults = append(csResults, pm.calculateChecksumWithFallback(dep))
    }
    pm.reportChecksumStats(csResults)
    pm.buildDependencyGraph()
    requestedBy := pm.calculateRequestedBy()

    module := entities.Module{
        Id:   fmt.Sprintf("%s:%s", pm.projectName, pm.projectVersion),
        Type: entities.ModuleType(pm.moduleType),
    }
    for i, dep := range pm.dependencies {
        cs := csResults[i]
        module.Dependencies = append(module.Dependencies, entities.Dependency{
            Id: dep.ID, Type: dep.Type, Scopes: dep.Scopes,
            RequestedBy: requestedBy[dep.ID],
            Checksum: entities.Checksum{Sha1: cs.SHA1, Sha256: cs.SHA256, Md5: cs.MD5},
        })
    }
    return &entities.BuildInfo{Modules: []entities.Module{module}}, nil
}
```

## Non-FlexPack Pattern
```go
func collectNonFlexPackBuildInfo(deps []parsedDep, checksums map[string]entities.Checksum, projectName, projectVersion, moduleType string) *entities.BuildInfo {
    module := entities.Module{
        Id:   projectName + ":" + projectVersion,
        Type: entities.ModuleType(moduleType),
    }
    for _, dep := range deps {
        cs := checksums[dep.name+":"+dep.version]
        module.Dependencies = append(module.Dependencies, entities.Dependency{
            Id: dep.name + ":" + dep.version, Type: dep.depType,
            Scopes: dep.scopes, Checksum: cs,
        })
    }
    return &entities.BuildInfo{Modules: []entities.Module{module}}
}
```

Module types: `"poetry"`, `"cargo"`, `"npm"`, `"maven"`, `"gradle"`, `"helm"`, `"conan"`, `"docker"`
