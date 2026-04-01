# Concurrent Worker Pool Template

```go
const aqlWorkerCount = 15

type aqlBatch struct {
    repo string
    deps []parsedDep
}

func batchedAQLFetch(batches []aqlBatch, sm artifactory.ArtifactoryServicesManager) map[string]entities.Checksum {
    results := make(map[string]entities.Checksum)
    var mu sync.Mutex
    var wg sync.WaitGroup
    sem := make(chan struct{}, aqlWorkerCount)

    for _, batch := range batches {
        wg.Add(1)
        go func(b aqlBatch) {
            defer wg.Done()
            sem <- struct{}{}
            defer func() { <-sem }()

            query := buildBatchAQLQuery(b.repo, b.deps)
            aqlResults, err := utils.ExecuteAqlQuery(sm, query)
            if err != nil {
                log.Warn("AQL batch failed for repo", b.repo, ":", err)
                return
            }
            mu.Lock()
            defer mu.Unlock()
            mapAQLResults(b.deps, aqlResults, results)
        }(batch)
    }
    wg.Wait()
    return results
}

func splitIntoBatches(deps []parsedDep, size int) [][]parsedDep {
    var batches [][]parsedDep
    for i := 0; i < len(deps); i += size {
        end := i + size
        if end > len(deps) { end = len(deps) }
        batches = append(batches, deps[i:end])
    }
    return batches
}
```
