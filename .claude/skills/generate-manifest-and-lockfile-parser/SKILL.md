---
name: generate-manifest-and-lockfile-parser
description: "Generates Go source code for parsing a package manager's manifest and lock files. Produces struct definitions, unmarshaling logic, and field extraction. Handles TOML, JSON, YAML, XML, and line-based formats."
version: 1.0.0
user-invocable: false
---

# Generate Manifest and Lock File Parser

## Input
- Package manager name
- Manifest format (toml/json/yaml/xml/line-based/dsl)
- Lock file format (toml/json/yaml/none)

## Procedure

### Step 1: Look up known struct definitions
Read `.claude/references/shared/manifest-and-lockfile-field-maps.md`.
If the PM is listed, use pre-built Go struct definitions directly.

### Step 2: For unknown PMs — discover fields
Follow `.claude/references/setup/field-discovery-procedure.md`.

### Step 3: Select parsing pattern by format
- TOML → `github.com/BurntSushi/toml` with `toml:"field"` tags
- JSON → `encoding/json` with `json:"field"` tags
- YAML → `gopkg.in/yaml.v3` with `yaml:"field"` tags
- XML → `encoding/xml` with `xml:"element"` tags
- Line-based → `bufio.Scanner` with regex
- DSL (Gradle) → regex extraction with comment stripping

### Step 4: Generate Go code
- Struct definitions with correct tags
- `loadManifest()` with error handling
- `loadLockFile()` (if applicable)
- Skip invalid entries, use defaults for missing data, fail fast for critical files

## Output
Go source code: type definitions, load functions, helpers
