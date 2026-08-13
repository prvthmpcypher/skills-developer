---
name: legacy-code-modernizer
description: >-
  Maps dependencies and debt hotspots in a legacy codebase and plans incremental migration with
  strangler-fig staging. Use when modernising without a rewrite. For the cutover, use
  migration-engineer.
---

# Legacy Code Modernizer

Systematically assesses legacy codebases and architects incremental modernization roadmaps without big-bang rewrites.

## Phased Workflow

### Phase 1: Codebase Archaeology & Risk Assessment
1. Map dependency graph (internal modules, external libraries, framework versions, EOL dates).
2. Identify technical debt hotspots using code churn × complexity heatmaps.
3. Classify components by migration risk: Low (pure logic), Medium (framework-coupled), High (data-layer entangled).

### Phase 2: Migration Strategy Design
1. Select pattern per component:
   - **Strangler Fig:** Gradually replace behind a routing facade.
   - **Branch by Abstraction:** Introduce abstraction layer, swap implementation beneath.
   - **Parallel Run:** Run old and new simultaneously, compare outputs, switch on confidence.
2. Define migration sequence: start with lowest-risk, highest-value components.
3. Establish contract tests at every boundary to prevent regression during transition.

### Phase 3: Incremental Execution & Verification
1. Migrate one component at a time with feature flags for instant rollback.
2. Validate each migration via automated diff testing, load testing, and canary deployment.
3. Decommission old code paths only after soak period validation.

## Verification & Quality Checklist
- [ ] Dependency audit complete with all EOL/CVE libraries flagged.
- [ ] Migration sequence prioritized by business value and technical risk.
- [ ] Contract tests cover all inter-module boundaries.
- [ ] Rollback mechanism (feature flags) tested for every migrated component.
- [ ] Zero data loss verified during data layer migrations.

## Anti-Patterns & Constraints
- NEVER attempt a full rewrite of a working production system in a single release.
- NEVER migrate without comprehensive test coverage on the existing system first.
- NEVER ignore backward compatibility for APIs consumed by external clients.
