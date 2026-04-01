# CLI Resolution Commands

| PM | Command | Output Format | Includes Transitives | Includes Scopes | Parsing Notes |
|---|---|---|---|---|---|
| **Cargo** | `cargo tree` | Text tree (`+---`) | Yes | No (infer from Cargo.toml) | Filter "Downloading:" lines; depth by leading spaces |
| **Poetry** | `poetry show --tree` | Text tree (│├└) | Yes | No (infer from pyproject.toml) | Strip tree chars; track depth by indentation |
| **pnpm** | `pnpm ls -r --depth Infinity --json` | JSON | Yes | Yes (deps vs devDeps) | Recursive struct; `resolved` field = tarball URL; skip `link:` versions |
| **Helm** | `helm dependency list <chart-dir>` | Text table | No (flat model) | N/A | Tab-separated columns; also `helm repo list` for alias resolution |
| **Conan** | `conan graph info . --format=json` | JSON | Yes | Yes (context field) | Conan 2.x only; nodes with context: host/build/test |
| **Gradle** | `gradle :mod:dependencies --configuration X` | Text tree (`+---\---`) | Yes | Yes (per-config) | `(*)` = skip subtree; `→`/`->` = version conflict; per-module, per-config |
| **Maven** | `mvn dependency:tree -DoutputType=json` | JSON | Yes | Yes (scope element) | Also supports text output; add `-B` for batch mode |
| **npm** | `npm ls --json --all` | JSON | Yes | Yes (dev flag) | Recursive dependencies map |
| **Go** | `go list -m -json all` | JSON stream | Yes | No (infer from imports) | One JSON object per line; use `go mod graph` for edges |
| **Docker** | N/A | N/A | N/A | N/A | Dependencies from FROM instructions + go-containerregistry for layers |

## Text Tree Parsing Cheat Sheet
```
Characters to strip: │ ├ └ ─ + | \ --- +---
Depth calculation: count leading whitespace / indent-per-level
Version conflict: look for → or -> (take the resolved version after arrow)
Duplicate marker: (*) means subtree already printed — skip
Status lines: "Downloading:", "Downloaded:", "Compiling:" — filter out
```
