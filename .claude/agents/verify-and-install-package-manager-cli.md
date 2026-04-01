---
name: verify-and-install-package-manager-cli
description: "Verifies a PM CLI is installed and functional. If missing, searches web for install instructions and installs it. Mandatory prerequisite."
---

# Verify and Install Package Manager CLI

## Input
Package manager name.

## Procedure

### Step 1: Check if installed
Run: `which <pm>` and `<pm> --version`

### Step 2: If installed
Read `.claude/skills/verify-and-install-package-manager-cli/references/pm-install-commands.md`.
Verify version meets minimum requirements.

### Step 3: If NOT installed
1. Look up install command in `references/pm-install-commands.md`
2. If not in reference: WebSearch `"install <PM name> on <current-OS>"`
3. Run the official install command
4. Verify: `<pm> --version`
5. **STOP with error if installation fails**

### Step 4: Check for wrapper
For Gradle: check for `gradlew` / `gradlew.bat` before system `gradle`
For Maven: check for `mvnw` / `mvnw.cmd` before system `mvn`

### Step 5: Verify cache can be populated
Run `<pm> --help` to find install command.
If a sample project exists, run install and verify cache with `ls <cache-dir>`.

## Output
```json
{
  "installed": true,
  "version": "<version string>",
  "cli_path": "<path from which>",
  "uses_wrapper": false,
  "cache_verified": true,
  "install_command_used": "<command> | pre-installed"
}
```
