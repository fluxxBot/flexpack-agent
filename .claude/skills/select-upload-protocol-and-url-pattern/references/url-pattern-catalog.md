# Artifactory URL Pattern Catalog

| PM | URL Pattern | Example |
|---|---|---|
| **PyPI/Poetry** | `{base}/api/pypi/{repo}/` | `https://art.example.com/api/pypi/pypi-local/` |
| **npm/pnpm** | `{base}/api/npm/{repo}/` | `https://art.example.com/api/npm/npm-local/` |
| **Maven** | `{base}/{repo}/{groupPath}/{artifact}/{version}/{filename}` | `https://art.example.com/maven-local/com/example/lib/1.0/lib-1.0.jar` |
| **Gradle** | Same as Maven | Same as Maven |
| **Helm (Classic)** | `{base}/api/helm/{repo}/` | `https://art.example.com/api/helm/helm-local/` |
| **Helm (OCI)** | `{base}/api/docker/{repo}/v2/` | `https://art.example.com/api/docker/helm-oci/v2/` |
| **Conan** | `{base}/api/conan/{repo}/v2/` | `https://art.example.com/api/conan/conan-local/v2/` |
| **Cargo** | `{base}/api/cargo/{repo}/` | `https://art.example.com/api/cargo/cargo-local/` |
| **Go** | `{base}/api/go/{repo}/` | `https://art.example.com/api/go/go-local/` |
| **Docker** | `{base}/api/docker/{repo}/v2/` | `https://art.example.com/api/docker/docker-local/v2/` |
| **Generic** | `{base}/{repo}/{path}` | `https://art.example.com/generic-local/path/to/file` |

## Deploy Path Construction per PM

| PM | Deploy Path Pattern | Scoped Example |
|---|---|---|
| **npm/pnpm** | `{name}/-/{name}-{version}.tgz` | `@scope/pkg/-/@scope/pkg-1.0.0.tgz` |
| **PyPI** | `{name}/{version}/{filename}` | `requests/2.31.0/requests-2.31.0-py3-none-any.whl` |
| **Maven** | `{groupPath}/{artifactId}/{version}/{artifactId}-{version}.{type}` | `com/google/guava/guava/31.1-jre/guava-31.1-jre.jar` |
| **Helm** | `{chartname}-{version}.tgz` | `postgresql-14.3.3.tgz` |
| **Conan** | `_/{name}/{version}/_/{revhash}/export` (recipe) | `_/zlib/1.2.13/_/abc123/export` |
