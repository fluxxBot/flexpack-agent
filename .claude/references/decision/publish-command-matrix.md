# Publish Command Matrix

| PM | Build Command | Publish Command | Variants | Artifact Format | Artifact Location | Protocol |
|---|---|---|---|---|---|---|
| **Poetry** | `poetry build` | `poetry publish` | | `.whl`, `.tar.gz` | `dist/` | HTTP POST multipart |
| **Cargo** | `cargo package` | `cargo publish` | | `.crate` | `target/package/` | HTTP PUT |
| **pnpm** | `pnpm pack` | `pnpm publish` | `pnpm publish -r --report-summary` (workspace) | `.tgz` | current dir / temp | HTTP PUT |
| **Helm (Classic)** | `helm package .` | HTTP PUT to Artifactory | | `.tgz` | current dir | HTTP PUT |
| **Helm (OCI)** | `helm package .` | `helm push <chart>.tgz oci://<registry>` | | OCI layers | OCI registry | OCI Push |
| **Conan** | N/A (recipe-based) | `conan upload <ref> -r <remote>` | `conan upload "*" -r <remote> -c` | recipe + package revisions | Conan cache | `conan upload` CLI |
| **Gradle** | `gradle build` | `gradle publish -I init-script.gradle` | `gradlew publish` | `.jar`, `.aar`, `.pom` | `build/libs/`, `build/publications/` | HTTP PUT (via init script) |
| **Maven** | `mvn package` | `mvn deploy` | `mvnw deploy` | `.jar`, `.war`, `.pom` | `target/` | HTTP PUT |
| **npm** | `npm pack` | `npm publish` | | `.tgz` | current dir | HTTP PUT |
| **Go** | N/A | N/A (GOPROXY protocol) | | `.zip`, `.info`, `.mod` | N/A | GOPROXY |
| **Docker** | `docker build` | `docker push` | `docker buildx build --push` | OCI layers | Registry | Docker Push |

## Publish-Specific Notes
- **pnpm workspace**: `pnpm publish -r --report-summary` produces `pnpm-publish-summary.json` — must remove before `pnpm pack`
- **pnpm deploy path**: `name/-/name-version.tgz` (both scoped and unscoped)
- **Helm dual**: detect via `oci://` prefix in repo URL
- **Conan upload parsing**: hierarchical indentation-based stdout; recipe revisions (32-char MD5) and package IDs (40-char SHA1)
- **Gradle init script**: embedded `init-artifact-extractor.gradle` collects artifacts with parallel ForkJoinPool checksums
- **Maven legacy vs modern**: legacy uses Java extractor JAR; modern uses pure Go with `mvn deploy` output parsing
- **Docker**: no separate build+upload; `docker push` handles upload. Build info collected AFTER push by querying Artifactory.
