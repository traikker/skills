---
name: context-definition
description: Create or refresh `<project-root>/.agent/CONTEXT.md` as a concise domain map (terms, relationships, canonical entry points) for fast agent navigation.
---

# Context Definition

Generate `<project-root>/.agent/CONTEXT.md`. It's a navigational hub — not a full
documentation dump. Goal: let an agent understand a complex system in one read,
then jump to the right source file.

## Staleness

Check existing `<project-root>/.agent/CONTEXT.md` for `last-modified` date. Compare to latest git
commit.

## Structure

```md
# {Repo Name}

{One or two sentence description of what this system does and why it exists.}

## Last Modified

YYYY-MM-DD

## Language

### {Subheading}

**Term**:
One-sentence definition of what it IS.
_Avoid_: Alias1, Alias2

### {Subheading}

...

## Relationships

- A **Term1** contains zero or more **Term2**
- A **Term3** belongs to exactly one **Term1**

## Entry Points

- **Course** → [`models/course.ts`](models/course.ts) — definition & lifecycle
- **Publish** → [`services/publish.ts`](services/publish.ts) — orchestrator

## Example dialogue

> **Dev:** "..."
> **Domain expert:** "..."

## Flagged ambiguities

- "Foo" was used to mean both **A** and **B** — resolved: these are distinct.
```

## Rules

- **Only project-specific concepts.** General programming terms (timeout, error
  handler, middleware) don't belong. Before adding a term, ask: is this unique to
  this codebase?
- **Be opinionated.** Pick the canonical term, list alternatives as `_Avoid_`.
- **One pointer per concept.** Link to the canonical source file. Skip generated
  code, vendor, tests, and config.
- **Keep definitions tight.** One sentence max. Define what it IS, not what it
  does.
- **Show cardinality.** Relationships use bold term names with explicit counts
  ("one or more", "exactly one").
- **Write dialogue.** At least one exchange demonstrating how terms interact.
- **Flag real ambiguities.** Only terms that are actively misused in the codebase.
  Resolve them in this file.

## Process

1. **Scan**: Read `AGENTS.md`, `README.md`, package.json entry points, and any
   existing `.agent/` to understand the project.
2. **Walk**: Explore source files to discover domain entities, modules, and their
   relationships. Focus on interfaces and naming patterns.
3. **Extract**: Identify domain-specific terms. Note aliases used in code that
   contradict each other.
4. **Reconcile** (if `<project-root>/.agent/CONTEXT.md` exists):
   - If last-modified is >2 weeks behind latest commit, flag it to the user.
   - For every term in the Language section, verify the linked file still exists
     and the description remains accurate.
   - For every Entry Point, verify the path still exists and the role hasn't
     shifted.
   - Walk primary source directories for routes, components, or utilities not
     yet mentioned.
   - Note removed references (deleted files, renamed modules).
5. **Write**: Produce `<project-root>/.agent/CONTEXT.md` following the structure above.
6. **Verify**: Cross-check that every route or public entry in the application
   directory is accounted for. Ensure no referenced path is stale. Confirm you
   haven't included generic tooling terms that belong in framework docs, not
   here.
7. **Timestamp**: Set `last-modified` to today's date.

## Cases

**Covers**:
- Initializing a new codebase that lacks `<project-root>/.agent/CONTEXT.md`
- Refreshing a stale `<project-root>/.agent/CONTEXT.md`
- Extracting domain language from an unfamiliar repo

**Does not cover**:
- Updating `<project-root>/.agent/CONTEXT.md` after code changes — that is a separate skill (commit-time update)
- Per-module context files — this creates a single repo-root hub
- Architecture deepening suggestions — see `architecture` skill for that
