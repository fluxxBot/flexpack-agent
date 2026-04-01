# Non-FlexPack File Layout

ALL code in jfrog-cli-artifactory ONLY. ZERO code in build-info-go.

```
jfrog-cli-artifactory/artifactory/commands/{pm}/
  ├── install.go       # Install command: run native → collect deps → fetch checksums → save BI
  ├── publish.go       # Publish command: build → upload → collect artifacts → tag props → save BI
  ├── depresolver.go   # Dependency resolution via CLI
  ├── checksums.go     # Two-tier: build cache → batched AQL
  ├── registry.go      # Registry URL parsing, repo extraction, remote→cache mapping
  ├── utils.go         # Helpers (tarball URL parsing, scoped package handling)
  └── {pm}_test.go     # Tests
```

Package name: `{pm}` (e.g., `package pnpm`)
No FlexPackManager interface — standalone command handlers.
Checksum functions take `servicesManager` param (server calls required).
