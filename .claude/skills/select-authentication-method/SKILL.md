---
name: select-authentication-method
description: "Selects the authentication strategy for a package manager's interaction with Artifactory. Determines auth type, config file, JFrog auth pattern, and special handling."
version: 1.0.0
user-invocable: false
---

# Select Authentication Method

## Input
- Package manager name

## Procedure

### Step 1: Look up PM in reference
Read `references/auth-config-matrix.md`. If found, return documented auth pattern.

### Step 2: For unknown PMs
WebSearch: `"<PM name> authentication configuration"` and `"<PM name> private registry setup"`

### Step 3: Determine JFrog-specific pattern
Most PMs use Bearer token or Basic auth with Artifactory. Note any special handling.

## Output Format
```json
{
  "package_manager": "<name>",
  "auth_type": "token | basic | oauth | cli-login",
  "config_file": "<path>",
  "jfrog_auth_type": "Bearer | Basic",
  "special_handling": "<notes or null>",
  "config_pattern_example": "<example config line>"
}
```
