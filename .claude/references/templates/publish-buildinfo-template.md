# Publish Build Info Template

```go
type UploadedArtifact struct {
    Name, Extension, RepoPath, ModuleName, ModuleType string
    SHA1, SHA256, MD5 string
}

func collectPublishBuildInfo(artifacts []UploadedArtifact, installBI *entities.BuildInfo, buildName, buildNumber string) *entities.BuildInfo {
    bi := installBI
    if bi == nil { bi = &entities.BuildInfo{} }

    for _, art := range artifacts {
        module := findOrCreateModule(bi, art.ModuleName, art.ModuleType)
        module.Artifacts = append(module.Artifacts, entities.Artifact{
            Name: art.Name, Type: art.Extension, Path: art.RepoPath,
            Checksum: entities.Checksum{Sha1: art.SHA1, Sha256: art.SHA256, Md5: art.MD5},
        })
    }
    bi.Name = buildName
    bi.Number = buildNumber
    bi.Started = time.Now().Format("2006-01-02T15:04:05.000-0700")
    return bi
}
```

## Deploy Path per PM
- npm/pnpm: `{name}/-/{name}-{version}.tgz` (scoped: `@scope/pkg/-/@scope/pkg-1.0.0.tgz`)
- PyPI: `{name}/{version}/{filename}`
- Maven: `{groupPath}/{artifactId}/{version}/{artifactId}-{version}.{type}`
- Helm: `{chartname}-{version}.tgz`
