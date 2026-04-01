# Upload Protocol Matrix

| PM | Protocol | Artifact Format | Pros | Cons |
|---|---|---|---|---|
| **PyPI/Poetry** | HTTP POST (multipart) | `.whl`, `.tar.gz` | Standard twine-compatible | Multipart encoding complexity |
| **npm/pnpm** | HTTP PUT | `.tgz` | Simple, widely supported | No built-in resume |
| **Maven** | HTTP PUT | `.jar`, `.pom`, `.war` | Standard, per-file | Many files per artifact (jar+pom+sources+javadoc) |
| **Gradle** | HTTP PUT (via init script) | `.jar`, `.aar`, `.pom` | Leverages Gradle's publish task | Requires init script injection |
| **Helm (Classic)** | HTTP PUT | `.tgz` | Simple single-file upload | Classic repos only |
| **Helm (OCI)** | OCI Push | OCI layers | Modern standard | Requires OCI registry support |
| **Conan** | `conan upload` CLI | Recipe + package revisions | Handles complex revision tracking | Must parse CLI output |
| **Cargo** | HTTP PUT | `.crate` | Simple | Less common Artifactory integration |
| **Go** | GOPROXY protocol | `.zip`, `.info`, `.mod` | No explicit publish needed | Proxy-based, not direct upload |
| **Docker** | Docker Push (OCI) | OCI layers + manifests | Native Docker workflow | Build info collected post-push |
