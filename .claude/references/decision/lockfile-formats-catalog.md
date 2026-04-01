# Lock File Formats Catalog

| PM | Lock File | Format | Has Transitives | Has Checksums | Key Fields |
|---|---|---|---|---|---|
| **Cargo** | `Cargo.lock` | TOML | Yes | Yes (`checksum`) | `[[package]]` with name, version, source, checksum, dependencies |
| **Poetry** | `poetry.lock` | TOML | Yes | No | `[[package]]` with name, version, description, category, dependencies |
| **pnpm** | `pnpm-lock.yaml` | YAML | Yes | Yes (integrity) | Complex content-addressable format — prefer `pnpm ls --json` instead |
| **Helm** | `Chart.lock` | YAML | No (flat) | No | `dependencies` array with name, version, repository |
| **Conan** | `conan.lock` | JSON | Yes | No | `requires`, `build_requires` arrays of ref strings |
| **npm** | `package-lock.json` | JSON | Yes | Yes (integrity) | `packages` map with version, resolved, integrity, dependencies |
| **Go** | `go.sum` | Line-based | Yes | Yes (h1: hash) | `module version h1:hash` per line |
| **Gradle** | **None** | N/A | N/A | N/A | No lock file — must use CLI |
| **Maven** | **None** | N/A | N/A | N/A | No lock file — must use CLI |
| **Docker** | **None** | N/A | N/A | N/A | No lock file |

## Notes
- **pnpm**: Lock file exists but is extremely complex to parse. The recommended approach is `pnpm ls -r --depth Infinity --json` instead.
- **Poetry**: `poetry.lock` may not be committed to the repo. System must work without it.
- **Helm**: `Chart.lock` is flat (no transitive deps by design).
- **Go**: `go.sum` contains checksums but not a full dependency tree. Use `go list -m -json all` for the tree.
