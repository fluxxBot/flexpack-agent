---
name: implement-install-side-buildinfo
description: "Implements install-side build-info collection. Generates parser, resolver, checksum calculator, scope classifier, graph builder, and build-info collector code."
---

# Implement Install-Side Build Info

You are an autonomous code generation agent. You receive analysis results and produce Go source code for install-side build-info collection.

## Input
- Package manager name
- Analysis results JSON (from analyze agent)

## Procedure

### Step 1: Generate Parser
Read `.claude/skills/generate-manifest-and-lockfile-parser/SKILL.md` and its `references/`.
Read `.claude/references/shared/manifest-and-lockfile-field-maps.md`.
Generate Go struct definitions and load functions for the PM's manifest and lock file.

### Step 2: Generate Resolver
Based on `analysis.resolution_strategy.primary`:
- "lockfile-direct" → read `.claude/skills/generate-lockfile-direct-resolver/` skill + references
- "cli-based" → read `.claude/skills/generate-cli-based-dependency-resolver/` skill + references
- "hybrid" → read `.claude/skills/generate-hybrid-fallback-resolver/` skill + references

### Step 3: Generate Checksum Calculator
Based on `analysis.checksum_approach.type`:
- "local-files" → read `.claude/skills/generate-local-file-checksum-calculator/` skill + references
- "server-aql" → read `.claude/skills/generate-serverside-aql-checksum-fetcher/` skill + references
THEN always read `.claude/skills/generate-checksum-graceful-degradation-chain/` for fallback wrapper.

### Step 4: Generate Scope Classifier
Read `.claude/skills/classify-dependency-scopes/` skill + references.

### Step 5: Generate Dependency Graph
Read `.claude/skills/generate-dependency-graph-with-requestedby/` skill + references.

### Step 6: Generate Build Info Collector
Read `.claude/skills/generate-install-buildinfo-collector/` skill + references.

### Step 7: Scaffold Structure
Based on architecture, launch ONE agent:
- FlexPack → `create-flexpack-implementation-structure`
- Non-FlexPack → `create-serverside-implementation-structure`

Assemble all generated code into the scaffolded files.

### Step 8: Apply Gotchas
Read `.claude/references/shared/known-pm-gotchas.md`. Apply BOTH PM-specific AND general gotchas.

## Critical Code Generation Rules

These rules are NON-NEGOTIABLE. Violations cause runtime failures.

1. **Working directory**: Always resolve to absolute path at the start of `Run()`:
   ```go
   if wd == "" || wd == "." {
       wd, _ = os.Getwd()
   } else if !filepath.IsAbs(wd) {
       wd, _ = filepath.Abs(wd)
   }
   ```

2. **Extra CLI flags**: Pass `args` (user's extra flags) to ALL PM subcommands — build, path-info, store dump, etc. Never assume a subcommand works without the same flags the build command needed.

3. **Build output capture**: Use the PM's default output mechanism (symlinks, result dirs). Read the symlink AFTER build completes. Do NOT parse stdout from `--print-out-paths` style flags.

4. **Three checksums**: Always compute sha1 + sha256 + md5. If the PM only provides one natively, compute the others by streaming the artifact through `io.MultiWriter(sha1.New(), md5.New())`.

5. **Command interface**: Every command struct MUST implement `Run()`, `ServerDetails()`, `CommandName()`. `ServerDetails()` can return `config.GetDefaultServerConf()` if not explicitly configured.

## Output
List of files created with full paths.
