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

Present the plan as structured markdown in the conversation. Use the template below.

Then ask the user:
- Does the story decomposition feel right? (too coarse / too fine)
- Are dependencies between stories accurate?
- Should any stories be merged or split?
- Is the tech guidance sufficient?

Iterate until the user approves.

If the user wants to save the plan, ask where to write it (e.g. `plan.md`) and write it to that path.

## Plan Template

```markdown
# Implementation Plan — [Feature Name]

Built from: requirements.md | Codebase: [repo path]

## Dependency overview

Stories in recommended implementation order. Stories marked with ← depend on earlier stories.

---

## Story 1: [Title]

**Covers:** Use case(s) #[numbers]
**Blocked by:** None

### Tech guidance
(Optional — include when the story needs architectural reminders)

- [ ] Task 1
- [ ] Task 2
- [ ] Task 3

---

## Story 2: [Title]

**Covers:** Use case(s) #[numbers]
**Blocked by:** Story 1

### Tech guidance

- [ ] Task 1
- [ ] Task 2

---

[Repeat for each story]
```

## Rules

- Every story must trace back to use case(s) in the requirements
- Acceptance criteria from requirements are implicitly carried by each story via its tasks
- Edge cases must appear as tasks in whichever story handles that concern
- The full story set must collectively satisfy every goal and acceptance criterion in the requirements
- Do not write files unless the user explicitly asks to save the plan
