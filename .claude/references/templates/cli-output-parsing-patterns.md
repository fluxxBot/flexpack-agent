# CLI Output Parsing Patterns

## Text Tree Parsing
```go
func (pm *PackManager) parseTextTree(output []byte) error {
    lines := strings.Split(string(output), "\n")
    depthStack := []string{fmt.Sprintf("%s:%s", pm.projectName, pm.projectVersion)}
    seen := make(map[string]bool)

    for _, line := range lines {
        if pm.isStatusLine(line) { continue } // Skip "Downloading:", etc.
        trimmed := strings.TrimSpace(line)
        if trimmed == "" { continue }

        depth := pm.calculateDepth(line)
        name, version := pm.parseTreeLine(trimmed)
        if name == "" { continue }

        depID := fmt.Sprintf("%s:%s", name, version)
        if seen[depID] { continue }
        seen[depID] = true

        // Track parent for graph
        if depth < len(depthStack) {
            depthStack = depthStack[:depth]
        }
        if len(depthStack) > 0 {
            parent := depthStack[len(depthStack)-1]
            pm.dependencyGraph[parent] = append(pm.dependencyGraph[parent], depID)
        }
        depthStack = append(depthStack, depID)

        pm.dependencies = append(pm.dependencies, utils.DependencyInfo{
            Type: pm.packageType, ID: depID, Name: name, Version: version,
        })
    }
    return nil
}

// Strip tree characters: │ ├ └ ─ + | \ --- +---
func (pm *PackManager) stripTreeChars(line string) string {
    r := strings.NewReplacer("│", " ", "├", " ", "└", " ", "─", " ",
        "+---", "    ", "\\---", "    ", "|", " ")
    return r.Replace(line)
}

// Handle version conflict markers: 1.0 -> 1.1 or 1.0 → 1.1
func (pm *PackManager) resolveVersionConflict(versionStr string) string {
    for _, sep := range []string{" -> ", " → "} {
        if parts := strings.SplitN(versionStr, sep, 2); len(parts) == 2 {
            return strings.TrimSpace(parts[1]) // Take resolved version
        }
    }
    return versionStr
}

// Skip duplicate markers: (*) means already printed
func (pm *PackManager) isDuplicate(line string) bool {
    return strings.HasSuffix(strings.TrimSpace(line), "(*)")
}

// Filter status lines
func (pm *PackManager) isStatusLine(line string) bool {
    prefixes := []string{"Downloading", "Downloaded", "Compiling", "Resolving"}
    for _, p := range prefixes {
        if strings.HasPrefix(strings.TrimSpace(line), p) { return true }
    }
    return false
}
```
