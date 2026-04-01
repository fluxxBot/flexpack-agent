# Publish-Side Build Info Implementation Checklist

## Phase 1: Analysis
- [ ] Select authentication method
- [ ] Select upload protocol and URL pattern
- [ ] Identify build/publish commands

## Phase 2: Core Implementation
- [ ] Implement auth configuration (token, basic, or PM-specific)
- [ ] Implement package build step (if PM requires explicit build before publish)
- [ ] Implement upload handler (HTTP PUT/POST/multipart or CLI-native)
- [ ] Implement artifact checksum computation on built artifacts
- [ ] Implement metadata extraction from manifest

## Phase 3: Build Info Collection
- [ ] Track uploaded artifacts (name, path, checksums)
- [ ] Tag build properties on artifacts in Artifactory (build.name, build.number, build.timestamp)
- [ ] Merge publish artifacts with install-side dependency info
- [ ] Generate complete BuildInfo with modules containing both artifacts and dependencies

## Phase 4: Validation
- [ ] Test build command produces expected artifacts
- [ ] Test upload to Artifactory repository
- [ ] Verify post-upload checksums match
- [ ] Verify build properties are set on artifacts
- [ ] Test with workspace/multi-module projects (if applicable)
- [ ] Run `go vet` and `go test` on generated code
