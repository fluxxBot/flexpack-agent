# Authentication Configuration Matrix

| PM | Default Auth | JFrog Auth | Config File | Special Handling |
|---|---|---|---|---|
| **PyPI/Poetry** | API Token | Bearer Token | `.pypirc` | Use `/api/pypi/` endpoint |
| **npm** | Auth Token | Bearer/Basic Token | `.npmrc` | Nerf dart URL matching (registry URL with trailing slash) |
| **pnpm** | `.npmrc` Auth Token | Bearer/Basic Token | `.npmrc` (project) + `~/.npmrc` (publish) | Same as npm; `pnpm publish` delegates to npm internally |
| **Maven** | Basic Auth | Basic/Token | `settings.xml` | Server ID matching in `<servers>` block |
| **Gradle** | Basic Auth | Basic/Token | `gradle.properties` or `build.gradle` | Repository-level credentials; also `~/.gradle/gradle.properties` |
| **Cargo** | Token | Bearer Token | `~/.cargo/credentials` | `[registries.<name>]` section with `token` field |
| **Helm (Classic)** | Basic Auth | Bearer Token | `helm repo add --username --password` | Per-repo credentials via CLI flags |
| **Helm (OCI)** | Registry Login | Bearer Token | `helm registry login` | **Must use `--password-stdin`** to prevent credential exposure in process args |
| **Conan** | Remote Login | Bearer Token | `conan remote login <remote> <user>` | Per-remote credentials; auto-login via `detectAndLoginToRemotes()` |
| **Go** | None/Token | Bearer Token | `GOPROXY` env var | `GONOSUMCHECK`, `GONOSUMDB` for private modules |
| **Docker** | Docker Login | Bearer Token | `docker login <registry>` | Credentials stored in `~/.docker/config.json` |
| **RubyGems** | API Key | Bearer Token | `~/.gem/credentials` | Key in HTTP `Authorization` header |

## Config Pattern Examples

```
# npm/pnpm .npmrc (nerf dart format)
//company.jfrog.io/artifactory/api/npm/npm-repo/:_authToken=<token>
registry=https://company.jfrog.io/artifactory/api/npm/npm-repo/

# Maven settings.xml
<server><id>artifactory</id><username>user</username><password>token</password></server>

# Cargo credentials
[registries.artifactory]
token = "Bearer <token>"

# Poetry (using environment variable)
POETRY_HTTP_BASIC_ARTIFACTORY_USERNAME=user
POETRY_HTTP_BASIC_ARTIFACTORY_PASSWORD=token
```
