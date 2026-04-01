# Graph Inversion for RequestedBy

```go
func (pm *PackManager) calculateRequestedBy() map[string][]string {
    requestedBy := make(map[string][]string)
    for parent, children := range pm.graph.edges {
        for _, child := range children {
            requestedBy[child] = append(requestedBy[child], parent)
        }
    }
    // Deduplicate and sort for deterministic output
    for child, requesters := range requestedBy {
        seen := make(map[string]bool)
        var unique []string
        for _, r := range requesters {
            if !seen[r] { seen[r] = true; unique = append(unique, r) }
        }
        sort.Strings(unique)
        requestedBy[child] = unique
    }
    return requestedBy
}
```

Diamond dependency example: libC requested by both libA and libB → `requestedBy["libC:1.0"] = ["libA:1.0", "libB:1.0"]`
