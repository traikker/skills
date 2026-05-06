---
name: define-requirements
description: Turn a brief or brainstorm summary into clear, testable requirements with use cases, acceptance criteria, edge cases, and dependencies.
---

# Define Requirements

Take a brainstorm result or brief idea and expand it into a requirements document detailed enough for an agent to execute without further human guidance.

## Process

1. **Locate the input** — look for a structured brainstorm summary in conversation, or whatever brief the user provides.

2. **Resolve ambiguities** — use the `grill-me` skill to relentlessly clarify open questions, assumptions, scope gaps, and edge cases. Keep asking until the requirements are unambiguous and complete. Document user decisions as they're made.

3. **Produce the requirements document** — output it inline. Only write to a file if the user explicitly requests one. If writing markdown, place it under `/<project-root>/.agent/` (default: `<project-root>/.agent/requirements.md`).

## Requirements Document Template

```markdown
# Requirements — [Feature Name]

## Overview

One-paragraph summary derived from the brainstorm.

## Goals

What this feature should achieve. 3-5 bullets.

## Non-Goals (Out of Scope)

What we explicitly will not do. Be specific.

## User Stories

Numbered stories in "As a <role>, I want <feature>, so that <benefit>" format.
Cover the happy path, common variations, and error cases.

## Use Cases

For each major flow, describe:
- **Trigger** — what initiates it
- **Steps** — actor and system interactions
- **Expected result** — successful outcome
- **Alternative paths** — branches or variations
- **Error scenarios** — what goes wrong and how we recover

## Acceptance Criteria

Testable conditions per user story or use case. Use Given/When/Then format.

## Edge Cases & Assumptions

- Edge cases the implementation must handle
- Assumptions made (especially those from unresolved open questions)

## Dependencies

- Other features, services, or external systems this depends on
```

## Writing Guidelines

- **Be specific enough to act on** — avoid vague language like "handle errors" without specifying which errors and how.
- **Make it testable** — every requirement should be verifiable.
- **Cover the negative path** — errors, edge cases, missing input, rate limits, etc.
- **Respect the brainstorm's scope** — don't add features the user explicitly excluded.
- **Default to conservative scope** — if in doubt, scope it narrower and document why.
