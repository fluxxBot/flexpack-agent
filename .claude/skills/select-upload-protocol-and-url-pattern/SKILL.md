---
name: select-upload-protocol-and-url-pattern
description: "Selects the upload protocol (HTTP PUT/POST/multipart/CLI-native) and Artifactory URL pattern for publishing packages."
version: 1.0.0
user-invocable: false
---

# Select Upload Protocol and URL Pattern

## Input
- Package manager name

## Procedure

### Step 1: Look up PM
Read `.claude/references/decision/upload-protocol-matrix.md` and `.claude/references/decision/url-pattern-catalog.md`.

### Step 2: For unknown PMs
WebSearch: `"<PM name> registry upload API"` and `"<PM name> artifactory integration"`

### Step 3: Check for dual protocol
Some PMs support multiple protocols (e.g., Helm: classic HTTP + OCI registry).

## Output Format
```json
{
  "package_manager": "<name>",
  "protocol": "HTTP PUT | HTTP POST | multipart | chunked | CLI-native | OCI Push",
  "artifact_format": ".<ext>",
  "url_pattern": "<Artifactory URL template>",
  "metadata_delivery": "inline | headers | separate",
  "dual_protocol": false,
  "secondary_protocol": null
}
```
