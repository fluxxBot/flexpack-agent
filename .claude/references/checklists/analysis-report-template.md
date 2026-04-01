# Analysis Report Template

```json
{
  "package_manager": "<name>",
  "architecture": {
    "type": "flexpack | non-flexpack",
    "code_locations": {
      "core": "<path or N/A>",
      "cli": "<path>",
      "tests": "<path>"
    },
    "rationale": "<why this architecture>"
  },
  "commands_to_intercept": {
    "install": {
      "primary": "<pm install command>",
      "variants": ["<all install aliases>"],
      "jfrog_wraps": "instead-of | after",
      "dependency_tree_command": "<pm tree command>",
      "tree_output_format": "json | text-tree"
    },
    "publish": {
      "build_command": "<pm build or null>",
      "publish_command": "<pm publish>",
      "artifact_format": ".<ext>",
      "artifact_location": "<path pattern>"
    }
  },
  "resolution_strategy": {
    "primary": "lockfile-direct | cli-based | hybrid",
    "manifest_file": "<filename>",
    "manifest_format": "toml | json | yaml | xml | line-based | dsl",
    "lock_file": "<filename or null>",
    "lock_format": "<format or null>",
    "handles_transitives": true
  },
  "checksum_approach": {
    "type": "local-files | server-aql",
    "priority_chain": ["<ordered sources>"],
    "file_extension": ".<ext> or N/A",
    "batch_config": {
      "batch_size": 30,
      "worker_count": 15
    }
  },
  "cache_paths": {
    "macos": "<path>",
    "linux": "<path>",
    "windows": "<path>",
    "env_override": "<VAR_NAME>"
  },
  "authentication": {
    "auth_type": "<type>",
    "config_file": "<path>",
    "jfrog_auth": "<Bearer | Basic>",
    "special_handling": "<notes>"
  },
  "upload_protocol": {
    "protocol": "<HTTP PUT | POST | CLI-native>",
    "url_pattern": "<Artifactory URL pattern>",
    "artifact_format": ".<ext>"
  },
  "complexity_score": {
    "parsing": "1-5",
    "cache": "1-5",
    "dependency": "1-5",
    "platform": "1-5",
    "overall": "1-5"
  },
  "recommended_template": "simple | cli-based | complex",
  "known_gotchas": ["<list from known-pm-gotchas.md>"]
}
```
