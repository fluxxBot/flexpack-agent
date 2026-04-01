# Install Command Matrix

| PM | Install Command | Variants | jfrog Wraps | Dependency Tree Command | Tree Format | Lock File | Cache After |
|---|---|---|---|---|---|---|---|
| **Poetry** | `poetry install` | `poetry add` | after | `poetry show --tree` | text-tree (│├└) | poetry.lock | Yes |
| **Cargo** | `cargo build` | `cargo fetch` | after | `cargo tree` | text-tree (+--) | Cargo.lock | Yes |
| **pnpm** | `pnpm install` | `pnpm i`, `pnpm add` | after | `pnpm ls -r --depth Infinity --json` | JSON | pnpm-lock.yaml | Yes (store) |
| **Helm** | `helm dependency update` | `helm dep up` | after | `helm dependency list` | text-table | Chart.lock | Yes |
| **Conan** | `conan install .` | `conan install . -of=build` | after | `conan graph info . --format=json` | JSON | conan.lock | Yes |
| **Gradle** | `gradle build` | `gradlew build` | after | `gradle :mod:dependencies --configuration runtimeClasspath` | text-tree (+---\|) | None | Yes |
| **Maven** | `mvn install` | `mvnw install` | after | `mvn dependency:tree -DoutputType=json` | JSON | None | Yes |
| **npm** | `npm install` | `npm i`, `npm ci` | after | `npm ls --json --all` | JSON | package-lock.json | Yes |
| **Go** | `go build` | `go mod download` | after | `go list -m -json all` | JSON (stream) | go.sum | Yes |
| **Docker** | `docker build` | `docker buildx build` | after | N/A (FROM instructions) | Dockerfile parse | None | No (layers on registry) |

## Tree Command Notes
- **Cargo**: output includes "Downloading:" lines that must be filtered
- **Poetry**: tree characters │├└ need stripping; depth tracked by indentation
- **Gradle**: markers `(*)` = duplicate (skip subtree), `→`/`->` = version conflict resolved, `(c)` = conflict, `(n)` = not in POM
- **Maven**: `-DoutputType=json` gives structured output; text output uses `+-` tree
- **pnpm**: `--depth Infinity` required for full transitive tree; `resolved` field contains tarball URL for AQL
- **Conan**: requires Conan 2.x; `--format=json` gives graph with nodes and contexts
