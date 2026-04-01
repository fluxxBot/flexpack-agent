# Artifact Property Tagging

```go
func tagBuildProperties(sm artifactory.ArtifactoryServicesManager, artifacts []UploadedArtifact, buildName, buildNumber, projectKey string) error {
    props := fmt.Sprintf("build.name=%s;build.number=%s;build.timestamp=%d",
        buildName, buildNumber, time.Now().UnixMilli())
    if projectKey != "" {
        props += fmt.Sprintf(";build.project=%s", projectKey)
    }
    for _, art := range artifacts {
        params := services.NewPropsParams()
        params.Pattern = art.RepoPath
        params.Props = props
        if err := sm.SetProps(params); err != nil {
            log.Warn("Failed to set props on", art.RepoPath, ":", err)
            // Don't fail — continue
        }
    }
    return nil
}
```

Uses `SetProps` API directly — avoids expensive `SearchFiles` API calls.
Properties: `build.name`, `build.number`, `build.timestamp`, optionally `build.project`.
