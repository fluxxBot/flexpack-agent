---
name: identify-commands-to-intercept-for-buildinfo
description: "Discovers which CLI commands must be intercepted by jfrog-cli for build-info collection. Runs --help, web searches, tests commands."
---

# Identify Commands to Intercept

You are an investigation agent. Discover the PM's CLI commands relevant to build-info.

## Input
Package manager name.

## Procedure

### Step 1: Check known PMs
Read `.claude/references/decision/install-command-matrix.md`.
Read `.claude/references/decision/publish-command-matrix.md`.
If listed, return documented commands.

### Step 2: Discover (unknown PMs only)
1. Run: `<pm> --help` — capture full output
2. WebSearch: `"<PM name> install dependencies command"`
3. WebSearch: `"<PM name> publish package command"`
4. WebSearch: `"<PM name> dependency tree command"`
5. Test the discovered commands on a sample project
6. For the tree command: run it and observe the output format (JSON? text tree?)

### Step 3: Determine wrapping strategy
For each intercepted command: does jfrog-cli run before, instead-of, or after?

## Output
```json
{
  "package_manager": "<name>",
  "install_side": {
    "primary_install_command": "...",
    "all_install_variants": [],
    "jfrog_wraps": "instead-of | after",
    "dependency_tree_command": "...",
    "tree_output_format": "json | text-tree",
    "lock_file_produced": "... | null",
    "cache_populated_after": true
  },
  "publish_side": {
    "build_command": "... | null",
    "publish_command": "...",
    "artifact_format": ".<ext>",
    "produces_artifact_at": "<path>"
  },
  "auxiliary": {
    "config_commands": [],
    "auth_commands": [],
    "workspace_flag": "... | null"
  }
}
```
