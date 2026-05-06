---
name: create-plan
description: Convert requirements into executable stories with tasks, dependencies, and targeted technical guidance for coding execution.
---

# Create Plan

Transform requirements provided by the user and codebase context into a detailed implementation plan of stories. Plan is delivered inline in the conversation.

## Process

### 1. Receive requirements

- If the user points to a requirements file (commonly `<project-root>/.agent/requirements.md`), read it
- Otherwise, accept the requirements from the conversation context or ask the user to paste them
- Internalise the full requirements — goals, non-goals, user stories, use cases, acceptance criteria, edge cases, and dependencies

### 2. Examine codebase context

- If the user specifies a repo path, scan it. Otherwise ask where the codebase lives.
- Check for `<project-root>/.agent/CONTEXT.md` — if present, read it for architectural navigational shortcuts
- Check `<project-root>/.agent/adr/` directory — read any ADRs for past architectural decisions
- Scan the repo structure briefly (top-level directories, key modules) to understand the existing shape of the code
- Note any patterns, conventions, or constraints that stories must respect

### 3. Draft stories from use cases

- Map each use case (or group of naturally related use cases) from the requirements into a **story**
- Related use cases can be combined into a single story when they share the same domain area or implementation concern
- Each story is self-contained and represents one implementable ticket/issue
- Size stories so they can be completed in a single `coding-mode` run without needing a follow-up continuation story just to finish the core implementation
- Prefer one story per user-visible outcome or bounded implementation concern
- Ensure the full set of stories collectively covers all requirements goals and acceptance criteria — nothing is missed

### 4. Define tasks and dependencies

- Within each story, list **tasks** as checklist items (`- [ ]`) that guide incremental implementation
- Identify any story dependencies — a story may depend on another story completing first
- Order stories so blockers come first

### 5. Add tech guidance where needed

- For stories that touch architectural boundaries, introduce new patterns, or risk drifting from best practices, include a **Tech guidance** section
- Reference specific ADRs, existing patterns, or `<project-root>/.agent/CONTEXT.md` entries
- Keep guidance concise — it supplements, not replaces, the story's tasks

### 6. Coverage check

Before presenting the plan, verify every acceptance criterion from the requirements maps to at least one story's tasks. If any criterion is uncovered, add a task to the appropriate story. This check is internal — do not output it unless the user asks.

### 7. Story size check

Before presenting the plan, verify every story is small enough to execute in one go.

Classify each candidate story internally as either:
- `fits-one-run` — small enough to complete in a single `coding-mode` run
- `split-required` — too large; must be decomposed before final output

A story is `split-required` if any of these are true:
- It delivers more than one distinct user-visible outcome
- It spans multiple loosely related domains, services, or architectural seams
- Its tasks imply multiple major implementation phases
- It lacks a clear test boundary or demoable completion point
- It would likely require more than 5–7 implementation tasks to describe properly
- It is unlikely to be fully implemented in one `coding-mode` pass

When a story is `split-required`:
- Split by use case, workflow phase, architectural seam, or risk boundary
- Preserve traceability so each smaller story still maps back to requirements coverage
- Re-check dependencies after splitting
- Do not include the oversized story in final output

### 8. Present the plan

Present the plan as a JSON object in the conversation. Use the schema below.

Then ask the user:
- Does the story decomposition feel right? (too coarse / too fine)
- Are dependencies between stories accurate?
- Should any stories be merged or split?
- Is the tech guidance sufficient?

Iterate until the user approves.

If the user wants to save the plan, ask where to write it (e.g. `plan.json`) and write it to that path. If they request markdown output, default to `<project-root>/.agent/` (for example `<project-root>/.agent/plan.md`).

## JSON Schema

Output a single JSON object with this structure:

```json
{
  "feature": "[Feature Name]",
  "source": ".agent/requirements.md",
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
      "executionFit": "fits-one-run",
      "demoBoundary": "User can complete X flow end-to-end",
      "scopeNotes": "Single bounded concern. Clear test boundary.",
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
      "executionFit": "fits-one-run",
      "demoBoundary": "Admin can perform Y action successfully",
      "scopeNotes": "Depends on S1 but remains independently completable.",
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
- `stories[].executionFit` — must be `"fits-one-run"` in final output
- `stories[].demoBoundary` — one-line statement of the story's clear done/demo boundary
- `stories[].scopeNotes` — short justification for why the story is small enough to execute in one go
- `stories[].blockedBy` — array of story IDs that must complete first; `[]` if none
- `stories[].techGuidance` — architectural reminder string or `null`

## Rules

- Every story must trace back to use case(s) in the requirements
- Every story must be small enough to complete in a single `coding-mode` run
- Prefer one user-visible outcome or one bounded implementation concern per story
- Final output may contain only stories with `executionFit: "fits-one-run"`
- If a candidate story is `split-required`, split it before presenting the plan
- Every story must include a clear `demoBoundary` and brief `scopeNotes`
- Acceptance criteria from requirements are implicitly carried by each story via its tasks
- Edge cases must appear as tasks in whichever story handles that concern
- The full story set must collectively satisfy every goal and acceptance criterion in the requirements
- If a story would exceed roughly 5–7 tasks, spans multiple subsystems, or has no clean completion/test boundary, split it
- `blockedBy` is an array of story IDs that must complete before this story, empty array if none
- `techGuidance` is a string or `null` if not needed
- Do not write files unless the user explicitly asks to save the plan
