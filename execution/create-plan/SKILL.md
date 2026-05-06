---
name: create-plan
description: Transform a requirements document into a detailed implementation plan of stories with tasks, dependencies, and architecture guidance. Use when user wants to create an implementation plan, turn requirements into stories, or needs a story-based breakdown of work from a requirements document.
---

# Create Plan

Transform requirements provided by the user and codebase context into a detailed implementation plan of stories. Plan is delivered inline in the conversation.

## Process

### 1. Receive requirements

- If the user points to a `requirements.md` file, read it
- Otherwise, accept the requirements from the conversation context or ask the user to paste them
- Internalise the full requirements — goals, non-goals, user stories, use cases, acceptance criteria, edge cases, and dependencies

### 2. Examine codebase context

- If the user specifies a repo path, scan it. Otherwise ask where the codebase lives.
- Check for `/agent/CONTEXT.md` in the repo root — if present, read it for architectural navigational shortcuts
- Check `agent/adr/` directory — read any ADRs for past architectural decisions
- Scan the repo structure briefly (top-level directories, key modules) to understand the existing shape of the code
- Note any patterns, conventions, or constraints that stories must respect

### 3. Draft stories from use cases

- Map each use case (or group of naturally related use cases) from the requirements into a **story**
- Related use cases can be combined into a single story when they share the same domain area or implementation concern
- Each story is self-contained and represents one implementable ticket/issue
- Ensure the full set of stories collectively covers all requirements goals and acceptance criteria — nothing is missed

### 4. Define tasks and dependencies

- Within each story, list **tasks** as checklist items (`- [ ]`) that guide incremental implementation
- Identify any story dependencies — a story may depend on another story completing first
- Order stories so blockers come first

### 5. Add tech guidance where needed

- For stories that touch architectural boundaries, introduce new patterns, or risk drifting from best practices, include a **Tech guidance** section
- Reference specific ADRs, existing patterns, or CONTEXT.md entries
- Keep guidance concise — it supplements, not replaces, the story's tasks

### 6. Coverage check

Before presenting the plan, verify every acceptance criterion from the requirements maps to at least one story's tasks. If any criterion is uncovered, add a task to the appropriate story. This check is internal — do not output it unless the user asks.

### 7. Present the plan

Present the plan as a JSON object in the conversation. Use the schema below.

Then ask the user:
- Does the story decomposition feel right? (too coarse / too fine)
- Are dependencies between stories accurate?
- Should any stories be merged or split?
- Is the tech guidance sufficient?

Iterate until the user approves.

If the user wants to save the plan, ask where to write it (e.g. `plan.json`) and write it to that path.

## JSON Schema

Output a single JSON object with this structure:

```json
{
  "feature": "[Feature Name]",
  "source": "requirements.md",
  "codebase": "[repo path]",
  "stories": [
    {
      "id": "S1",
      "title": "[Title]",
      "covers": [1, 2],
      "tasks": [
        "Task 1",
        "Task 2",
        "Task 3"
      ],
      "blockedBy": [],
      "techGuidance": null
    },
    {
      "id": "S2",
      "title": "[Title]",
      "covers": [3],
      "tasks": [
        "Task 1",
        "Task 2"
      ],
      "blockedBy": ["S1"],
      "techGuidance": "Reference ADR-004 for event bus pattern"
    }
  ]
}
```

### Field definitions
- `feature` — short name of the feature
- `source` — path to source requirements file
- `codebase` — path to the scanned repository
- `stories` — ordered array of stories (blockers first)
- `stories[].id` — unique identifier like `S1`, `S2`
- `stories[].title` — one-line story title
- `stories[].covers` — array of use case numbers from requirements
- `stories[].tasks` — actionable steps, ordered by implementation sequence
- `stories[].blockedBy` — array of story IDs that must complete first; `[]` if none
- `stories[].techGuidance` — architectural reminder string or `null`

## Rules

- Every story must trace back to use case(s) in the requirements
- Acceptance criteria from requirements are implicitly carried by each story via its tasks
- Edge cases must appear as tasks in whichever story handles that concern
- The full story set must collectively satisfy every goal and acceptance criterion in the requirements
- `blockedBy` is an array of story IDs that must complete before this story, empty array if none
- `techGuidance` is a string or `null` if not needed
- Do not write files unless the user explicitly asks to save the plan
