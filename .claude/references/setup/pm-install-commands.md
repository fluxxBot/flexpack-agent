# PM Install Commands

| PM | Check | Install (macOS) | Install (Linux) | Min Ver |
|---|---|---|---|---|
| Cargo | `cargo --version` | `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs \| sh` | same | 1.70+ |
| Poetry | `poetry --version` | `curl -sSL https://install.python-poetry.org \| python3 -` | same | 1.5+ |
| pnpm | `pnpm --version` | `npm i -g pnpm` or `brew install pnpm` | `npm i -g pnpm` | 8.0+ |
| Helm | `helm version` | `brew install helm` | `curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 \| bash` | 3.0+ |
| Conan | `conan --version` | `pip install conan` | `pip install conan` | **2.0+** (1.x NOT supported) |
| Gradle | `gradle --version` | `brew install gradle` | `sdk install gradle` | 7.0+ |
| Maven | `mvn --version` | `brew install maven` | `sdk install maven` | 3.6+ |
| Go | `go version` | `brew install go` | download from golang.org | 1.19+ |
| Docker | `docker --version` | Docker Desktop | `curl -fsSL https://get.docker.com \| sh` | 20.10+ |

**Notes**: Gradle/Maven — check for wrapper (`gradlew`/`mvnw`) before system install. Conan MUST be 2.x.
