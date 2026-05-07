---
name: architecture-diagrams
description: Maintains D2 architecture diagrams from repository file and interface structure after coding changes. Use when coding tasks change files, module boundaries, exports, imports, dependencies, or public interfaces and `.agent/diagrams/modules.d2`, `.agent/diagrams/relationships.d2`, and `.agent/diagrams/components.d2` must be refreshed.
---

# Architecture Diagrams

## Quick start

When structural code changes land:
1. Inspect changed files and dependency/interface changes
2. Rebuild all diagrams:
   - `.agent/diagrams/modules.d2`
   - `.agent/diagrams/relationships.d2`
   - `.agent/diagrams/components.d2`
3. Keep diagrams consistent with current code

## Triggers

Run after coding work when any of these changed:
- file layout
- module boundaries
- imports/exports
- dependency edges
- interface/type/API contracts

## Workflow

### 1. Detect structure
Collect:
- added/removed/renamed files
- changed imports/dependencies
- changed exported symbols
- changed interfaces/contracts between modules

### 2. Rebuild modules diagram
Update `.agent/diagrams/modules.d2` to show:
- top-level modules/subsystems
- ownership/grouping by directory or package
- major public entry points

### 3. Rebuild relationships diagram
Update `.agent/diagrams/relationships.d2` to show:
- dependency edges between modules
- direction of use
- key interface boundaries
- external systems/packages if architecturally relevant

### 4. Rebuild components diagram
Update `.agent/diagrams/components.d2` to show:
- main runtime/build components
- internal responsibilities
- interfaces connecting components

## Rules

- Always refresh all 3 diagrams on qualifying changes
- Derive diagrams from code structure, not stale docs
- Prefer stable architectural abstractions over low-value detail
- Keep naming consistent with code
- Omit trivial utilities unless structurally important
- If structure is ambiguous, note assumption inline in D2 comments

## Output standard

For each `.d2` file:
- valid D2 syntax
- concise labels
- consistent terminology
- only meaningful nodes/edges
- comments for non-obvious inference
