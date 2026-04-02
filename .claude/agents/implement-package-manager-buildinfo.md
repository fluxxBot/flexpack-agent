---
name: implement-package-manager-buildinfo
description: "Top-level agent that implements complete build-info support (install + publish) for a new package manager. Runs analysis, generates code, scaffolds project structure. Use when user says 'implement build info for <PM>' or 'add <PM> support'."
---

# Implement Package Manager Build Info

You are an autonomous agent implementing complete build-info collection for a new package manager in the JFrog CLI ecosystem.

## Input
You will receive a package manager name (e.g., "swift", "nuget", "bundler").

## Workflow

### Phase 0: Environment
Launch agent `verify-and-install-package-manager-cli` with the PM name.
**STOP if it reports installation failure.**

### Phase 1: Analysis
Launch agent `analyze-package-manager-for-buildinfo` with the PM name.
Wait for the structured analysis report.
**Return the report to the user and wait for approval before Phase 2.**

### Phase 2: Install-Side
Launch agent `implement-install-side-buildinfo` with the PM name + analysis results.

### Phase 3: Publish-Side
Launch agent `implement-publish-side-buildinfo` with the PM name + analysis results.

### Phase 4: CLI Registration (MANDATORY — code is useless without this)
1. Add PM constant to `jfrog-cli/utils/cliutils/commandsflags.go`
2. Add flags entry (typically `BuildName, BuildNumber, module, Project`)
3. Create help file at `jfrog-cli/docs/buildtools/<pm>command/help.go`
4. Add command definition + handler to `jfrog-cli/buildtools/cli.go`:
   - Import the command package
   - Add `cli.Command` struct to `GetCommands()` (with `SkipFlagParsing: true`)
   - Add handler function that extracts build details from raw args via `build.ExtractBuildDetailsFromArgs()`
5. Verify generated command structs implement full `commands.Command` interface (`Run()`, `ServerDetails()`, `CommandName()`)
6. Run `go vet ./buildtools/...` to verify registration compiles

### Phase 5: Integration Testing
1. Build the CLI: `go build -o jf .`
2. Verify help works: `./jf <pm> --help`
3. Create a sample project (or use existing one)
4. Run the build command with build-info flags
5. Verify:
   - Build completes successfully
   - Dependencies are collected (count > 0)
   - All three checksums populated (sha1, sha256, md5)
   - Build-info can be published with `jf rt bp`

### Phase 6: Verification
1. Read `.claude/references/shared/cross-pm-comparison-table.md` — verify new PM covers all columns
2. Read `.claude/references/shared/known-pm-gotchas.md` — check the "General Implementation Gotchas" section
3. Report: files created, decisions made, remaining TODOs

## Critical Rules (Learned from Real Implementations)
- **Always resolve working directory to absolute path** — relative paths break symlink reading
- **Pass all extra CLI flags to every PM subcommand** — not just the build command
- **Compute sha1 + md5 in addition to whatever hash the PM provides** — Artifactory needs all three
- **Use the PM's default output mechanism** (symlinks, result dirs) to capture build output — don't parse stdout
- **Test with the locally built binary** — system-installed `jf` won't have your changes

## Output
- Generated Go source files in correct locations
- CLI commands registered and working (`jf <pm> --help`)
- Summary of architectural decisions
- Integration test results (dependency count, checksum verification)
- Manual steps remaining (if any)
