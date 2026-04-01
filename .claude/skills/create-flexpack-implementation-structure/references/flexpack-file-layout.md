# FlexPack File Layout

## Simple PM (Poetry, Cargo, Helm)
```
build-info-go/flexpack/
  └── {pm}_flexpack.go              # Struct + 5 interface methods

build-info-go/flexpack/tests/unit/
  └── {pm}_flexpack_test.go

jfrog-cli-artifactory/artifactory/commands/flexpack/
  └── {pm}.go                       # CLI handler, flag parsing
```

## Complex PM (Conan, Gradle, Maven)
```
build-info-go/flexpack/{pm}/
  ├── {pm}_flexpack.go              # Main implementation
  ├── {pm}_dependencies.go          # Parsing logic
  ├── {pm}_types.go                 # Type definitions
  └── {pm}_utils.go                 # Helpers

jfrog-cli-artifactory/artifactory/commands/flexpack/{pm}/
  ├── {pm}.go                       # CLI handler
  ├── {pm}_repo_finder.go           # Repository discovery
  └── {pm}_properties.go            # Build properties
```

## Naming
- Struct: `{Pm}FlexPack` (e.g., `CargoFlexPack`)
- Constructor: `New{Pm}FlexPack(config)`
- Package: `flexpack` (or PM name if subdirectory)
- DO NOT create new top-level directories
