# Install-Side Build Info Implementation Checklist

## Phase 0: Project Organization
- [ ] Identify existing directory structure (DO NOT create new top-level dirs)
- [ ] Create sample project in separate directory (if needed)
- [ ] Verify workspace follows existing patterns

## Phase 1: Analysis
- [ ] Determine FlexPack vs Non-FlexPack architecture
- [ ] Identify CLI commands to intercept
- [ ] Select dependency resolution strategy
- [ ] Select checksum computation approach
- [ ] Discover cache directories (FlexPack only)

## Phase 2: Core Implementation
- [ ] Implement manifest/lock file parser with correct struct definitions
- [ ] Implement dependency resolver (lockfile-direct, CLI-based, or hybrid)
- [ ] Implement checksum calculator with graceful degradation chain
- [ ] Implement scope classifier
- [ ] Implement dependency graph with RequestedBy inversion

## Phase 3: Integration
- [ ] Implement build-info collector (FlexPackManager interface or standalone handler)
- [ ] Wire into CLI command registration
- [ ] Add build property tagging

## Phase 4: Validation
- [ ] Test with real dependency trees (100+ dependencies)
- [ ] Verify cross-platform cache discovery
- [ ] Test error recovery scenarios
- [ ] Verify checksum success rate (target: >90% authentic)
- [ ] Performance test with large dependency sets
- [ ] Run `go vet` and `go test` on generated code
