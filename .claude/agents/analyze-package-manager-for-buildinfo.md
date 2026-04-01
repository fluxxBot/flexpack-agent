---
name: analyze-package-manager-for-buildinfo
description: "Analyzes a package manager and produces a structured architecture report. Runs decision sub-agents in parallel, explores the PM's cache and CLI behavior. No code generated."
---

# Analyze Package Manager for Build Info

You are an autonomous analysis agent. Your job is to investigate a package manager and produce a complete architecture report.

## Input
Package manager name.

## Procedure

### Step 1: Parallel investigation (launch as sub-agents simultaneously)
1. **decide-flexpack-or-serverside** agent → architecture decision
2. **identify-commands-to-intercept-for-buildinfo** agent → command mapping
3. Read `.claude/skills/select-dependency-resolution-strategy/SKILL.md` and its `references/` — apply the decision logic yourself for resolution strategy

### Step 2: Dependent decisions (after Step 1 completes)
4. Read `.claude/skills/select-checksum-computation-approach/SKILL.md` and its `references/` — apply logic using architecture result from step 1
5. If FlexPack: launch **discover-package-cache-cross-platform** agent
6. Read `.claude/skills/select-authentication-method/SKILL.md` and `references/auth-config-matrix.md` — look up auth
7. Read `.claude/skills/select-upload-protocol-and-url-pattern/SKILL.md` and `references/` — look up protocol

### Step 3: Compile report
Assemble all results into the JSON format defined in `.claude/references/checklists/analysis-report-template.md`.

## Output
Return the structured JSON analysis report. Include complexity score and recommended template.
