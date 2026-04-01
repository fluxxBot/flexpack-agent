# Graph Construction Patterns

## Adjacency List (Default)
```go
type DependencyGraph struct {
    edges map[string][]string
    nodes map[string]bool
}

func (dg *DependencyGraph) AddDependency(parent, child string) {
    dg.edges[parent] = append(dg.edges[parent], child)
    dg.nodes[parent] = true
    dg.nodes[child] = true
}
```

## Top-Down (from CLI tree output)
```go
func (pm *PackManager) buildGraphFromTree(lines []string) {
    root := fmt.Sprintf("%s:%s", pm.projectName, pm.projectVersion)
    stack := []string{root}
    for _, line := range lines {
        depth := pm.calculateDepth(line)
        name, ver := pm.parseTreeLine(line)
        key := name + ":" + ver
        if depth < len(stack) { stack = stack[:depth] }
        parent := stack[len(stack)-1]
        pm.graph.AddDependency(parent, key)
        stack = append(stack, key)
    }
}
```

## Bottom-Up (from lock file)
```go
func (pm *PackManager) buildGraphFromLock() {
    for _, pkg := range pm.lockFileData.Packages {
        pkgKey := pkg.Name + ":" + pkg.Version
        for _, depName := range pkg.Dependencies {
            depVer := pm.resolveVersion(depName)
            pm.graph.AddDependency(pkgKey, depName+":"+depVer)
        }
    }
}
```
