# AQL Query Patterns

## Standard Batch Query
```
items.find({
  "repo": "<repo-name>",
  "$or": [
    {"$and": [{"path": "<dirPath1>"}, {"name": "<fileName1>"}]},
    {"$and": [{"path": "<dirPath2>"}, {"name": "<fileName2>"}]},
    ...up to 30 entries per batch...
  ]
}).include("repo", "path", "name", "actual_sha1", "sha256", "actual_md5")
```

## Batch Configuration
- **Batch size**: 30 dependencies per AQL query
- **Worker pool**: 15 concurrent goroutines
- **Semaphore**: `make(chan struct{}, 15)` to limit concurrency

## Repo Resolution
- **Local repos**: query as-is
- **Remote repos**: add `-cache` suffix (`npm-remote` → `npm-remote-cache`)
- **Virtual repos**: query the underlying default deployment repo

```go
func resolveAqlRepo(repo string, sm artifactory.ArtifactoryServicesManager) string {
    repoDetails, _ := sm.GetRepository(repo)
    if repoDetails.Rclass == "remote" {
        return repo + "-cache"
    }
    return repo
}
```

## Tarball URL Parsing (pnpm example)
```
URL: "https://company.jfrog.io/artifactory/api/npm/npm-remote/xml/-/xml-1.0.1.tgz"
→ repo: "npm-remote"
→ dirPath: "xml/-"
→ fileName: "xml-1.0.1.tgz"

Scoped: "https://company.jfrog.io/artifactory/api/npm/npm-remote/@scope/pkg/-/pkg-1.0.0.tgz"
→ repo: "npm-remote"
→ dirPath: "@scope/pkg/-"
→ fileName: "pkg-1.0.0.tgz"
```

## Registry URL → Repo Name Extraction
```go
func extractRepoFromRegistryURL(registryURL string) string {
    idx := strings.Index(registryURL, "api/npm/")
    if idx == -1 { return "" }
    rest := registryURL[idx+len("api/npm/"):]
    return strings.TrimSuffix(rest, "/")
}
```
