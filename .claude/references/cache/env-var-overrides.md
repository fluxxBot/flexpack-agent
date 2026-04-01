# Environment Variable Overrides

Env vars take highest priority — check BEFORE platform-specific paths.

| PM | Env Var | Path Construction |
|---|---|---|
| Cargo | `CARGO_HOME` | `{CARGO_HOME}/registry/cache/` |
| Poetry | `POETRY_CACHE_DIR` | Use directly |
| Helm | `HELM_REPOSITORY_CACHE` | Use directly |
| Maven | `M2_HOME` | `{M2_HOME}/repository/` |
| Gradle | `GRADLE_USER_HOME` | `{GRADLE_USER_HOME}/caches/modules-2/files-2.1/` |
| Conan | `CONAN_HOME` | `{CONAN_HOME}/p/` (still needs `conan cache path`) |
| npm | `npm_config_cache` | Use directly |
| pnpm | `PNPM_HOME` | `{PNPM_HOME}/store/v3/` |
| Go | `GOMODCACHE` | Use directly (overrides GOPATH) |
| Go | `GOPATH` | `{GOPATH}/pkg/mod/cache/download/` |
| RubyGems | `GEM_HOME` | `{GEM_HOME}/cache/` |

## Go Code Pattern
```go
func (pm *PackManager) getCacheDirectories() []string {
    var paths []string
    if envPath := os.Getenv(pm.cacheEnvVar); envPath != "" {
        paths = append(paths, envPath)
    }
    switch runtime.GOOS {
    case "darwin":
        paths = append(paths, pm.macOSPaths...)
    case "windows":
        paths = append(paths, pm.windowsPaths...)
    default:
        paths = append(paths, pm.linuxPaths...)
    }
    return paths
}
```
