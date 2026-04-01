# CLI Resolver Template

```go
func (pm *PackManager) resolveWithCLI() error {
    args := append([]string{}, pm.resolverArgs...)
    cmd := exec.Command(pm.cliPath, args...)
    cmd.Dir = pm.config.WorkingDirectory
    cmd.Env = append(os.Environ(), pm.getEnvVars()...)

    var stdout, stderr bytes.Buffer
    cmd.Stdout = &stdout
    cmd.Stderr = &stderr

    // Set timeout
    ctx, cancel := context.WithTimeout(context.Background(), 5*time.Minute)
    defer cancel()
    cmd = exec.CommandContext(ctx, pm.cliPath, args...)
    cmd.Dir = pm.config.WorkingDirectory
    cmd.Stdout = &stdout
    cmd.Stderr = &stderr

    if err := cmd.Run(); err != nil {
        return fmt.Errorf("CLI resolution failed: %w\nStderr: %s", err, stderr.String())
    }

    return pm.parseCliOutput(stdout.Bytes())
}
```

## JSON Output Parsing
```go
func (pm *PackManager) parseJsonTree(data []byte) error {
    var tree CliTreeOutput
    if err := json.Unmarshal(data, &tree); err != nil {
        return err
    }
    seen := make(map[string]bool)
    pm.walkJsonTree(tree.Dependencies, seen, []string{"runtime"})
    return nil
}

func (pm *PackManager) walkJsonTree(deps map[string]CliDep, seen map[string]bool, scopes []string) {
    for name, info := range deps {
        if strings.HasPrefix(info.Version, "link:") { continue }
        depID := fmt.Sprintf("%s:%s", name, info.Version)
        if seen[depID] { continue }
        seen[depID] = true
        pm.dependencies = append(pm.dependencies, utils.DependencyInfo{
            Type: pm.packageType, ID: depID, Name: name,
            Version: info.Version, Scopes: scopes,
        })
        if info.Dependencies != nil {
            pm.walkJsonTree(info.Dependencies, seen, []string{"transitive"})
        }
    }
}
```
