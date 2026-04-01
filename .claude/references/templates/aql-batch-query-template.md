# AQL Batch Query Template (Tier 2)

```go
const aqlBatchSize = 30

func buildBatchAQLQuery(repo string, deps []parsedDep) string {
    var orClauses []string
    for _, d := range deps {
        orClauses = append(orClauses, fmt.Sprintf(
            `{"$and":[{"path":"%s"},{"name":"%s"}]}`, d.dirPath, d.fileName))
    }
    return fmt.Sprintf(
        `items.find({"repo":"%s","$or":[%s]}).include("repo","path","name","actual_sha1","sha256","actual_md5")`,
        repo, strings.Join(orClauses, ","))
}

func resolveAqlRepo(repo string, sm artifactory.ArtifactoryServicesManager) string {
    repoDetails, err := sm.GetRepository(repo)
    if err != nil { return repo }
    if repoDetails.Rclass == "remote" { return repo + "-cache" }
    return repo
}

func groupByRepo(deps []parsedDep) map[string][]parsedDep {
    groups := make(map[string][]parsedDep)
    for _, d := range deps {
        groups[d.repo] = append(groups[d.repo], d)
    }
    return groups
}
```
