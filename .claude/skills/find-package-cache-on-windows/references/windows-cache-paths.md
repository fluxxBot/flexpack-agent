# Windows Cache Paths

| PM | Primary Path | Env Override | File Pattern | Extension |
|---|---|---|---|---|
| Cargo | `%USERPROFILE%\.cargo\registry\cache\{hash}\` | `CARGO_HOME` | `{name}-{version}.crate` | `.crate` |
| Poetry | `%APPDATA%\pypoetry\Cache\artifacts\` | `POETRY_CACHE_DIR` | `{name}-{version}*.whl` | `.whl`, `.tar.gz` |
| Helm | `%APPDATA%\helm\repository\cache\` | `HELM_REPOSITORY_CACHE` | `{name}-{version}.tgz` | `.tgz` |
| Maven | `%USERPROFILE%\.m2\repository\{groupPath}\{artifactId}\{version}\` | `M2_HOME` | `{artifactId}-{version}.jar` | `.jar` |
| Gradle | `%USERPROFILE%\.gradle\caches\modules-2\files-2.1\{group}\{artifact}\{version}\{hash}\` | `GRADLE_USER_HOME` | `{artifact}-{version}.jar` | `.jar` |
| Conan | `%USERPROFILE%\.conan2\p\{pkgname}{hash}\` | `CONAN_HOME` | archives | `.tgz` |
| npm | `%APPDATA%\npm-cache\_cacache\content-v2\` | `npm_config_cache` | content-addressable | N/A |
| pnpm | `%LOCALAPPDATA%\pnpm\store\v3\` | `PNPM_HOME` | NO tarballs | N/A |
| Go | `%USERPROFILE%\go\pkg\mod\cache\download\` | `GOPATH`, `GOMODCACHE` | `{module}\@v\{version}.zip` | `.zip` |
| RubyGems | `%USERPROFILE%\.gem\ruby\{ruby-version}\cache\` | `GEM_HOME` | `{name}-{version}.gem` | `.gem` |
