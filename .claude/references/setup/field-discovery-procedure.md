# Field Discovery Procedure for Unknown Package Managers

When the PM is not in the known struct definitions, follow these steps:

## Step 1: Create Sample Project
```bash
mkdir sample-<pm>-project && cd sample-<pm>-project
<pm> init  # or equivalent initialization command
```

## Step 2: Add Dependencies
```bash
<pm> add <popular-package>  # Add a well-known package
<pm> add <another-package>  # Add a second for variety
```

## Step 3: Run Install (generates lock file)
```bash
<pm> install  # or equivalent
```

## Step 4: Inspect Manifest File
Read the manifest (package.json, Cargo.toml, pyproject.toml, etc.):
- Identify dependency sections (dependencies, devDependencies, etc.)
- Note the format (TOML, JSON, YAML, XML)
- Identify project metadata fields (name, version)

## Step 5: Inspect Lock File
Read the lock file (if generated):
- Identify package entry structure (name, version, checksum, dependencies)
- Note whether transitives are included
- Note whether checksums are present

## Step 6: WebSearch for Specification
```
WebSearch: "<PM name> lock file format specification"
WebSearch: "<PM name> manifest file fields"
```

## Step 7: Build Go Struct
From the observed fields, create Go struct definitions:
```go
type ManifestData struct {
    // Map observed fields to Go types with correct tags
    Name    string                 `<format>:"name"`
    Version string                 `<format>:"version"`
    Deps    map[string]interface{} `<format>:"dependencies"`
}
```

Test by writing a small Go program that unmarshals the real file into the struct.
