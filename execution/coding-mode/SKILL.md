---
name: coding-mode
description: Run end-to-end feature delivery. branch from main, implement via `write-code`, keep tests green, update `<project-root>/.agent/CONTEXT.md`, then squash-merge to main.
---

# Coding Mode

Activation: when user wants to implement a story end-to-end or says "coding mode"
- Story may be inline in the prompt or prompted for interactively
- Story can contain a checklist of subtasks or describe a single task

Loop:

## Phase 1 — Setup
1. If story not provided inline, ask user to share the story description
2. Capture the story (and any checklist items)
3. Derive branch slug from story title: lowercase, spaces to dashes, strip special chars
4. `git checkout main` then `git pull --rebase` (always pull fresh)
5. Create and checkout the feature branch

## Phase 2 — Implementation
1. For each checklist item (or the single story if no checklist):
   - Execute the `write-code` skill to implement the code
   - Ensure all tests pass after each item
   - Ensure `<project-root>/.agent/CONTEXT.md` is updated per `write-code` discipline
2. After all items complete, run full test suite — fix until green

## Phase 3 — Check
1. Read [`coding-standards`](../coding-standards/SKILL.md) and verify all standards are met
2. Run full test suite — must be green
3. If any check fails, return to Phase 2 to fix

## Phase 4 — Commit
1. Stage all changed files with `git add -A`
2. Draft one commit — title describes the change, body has implementation details
3. `git commit`

## Phase 5 — Merge
1. `git checkout main` then `git pull --rebase` (always pull fresh before merge)
2. Squash merge: `git merge --squash <branch>` followed by `git commit` with same message
3. Auto-resolve any merge conflicts using best effort
4. Delete the feature branch
5. Confirm merge is complete

Exit condition:
- All tests passing, branch merged to main, no uncommitted changes
- Stay in mode until merge completes — do not exit early
- Do not diverge — gently steer back if user changes topic mid-flow

Rules:
- Always pull fresh into main before branching and before merging
- No external scripts — use only available tools
- Auto-resolve merge conflicts with best effort
- If blocked on implementation, proceed with best recommendation and resume

## Quick Start

```
User: Add user signup endpoint
Agent: [creates branch `add-user-signup-endpoint`, runs write-code per checklist, tests green, squashes to main]
```

## Cases

**Covers**:
- Single story with or without a subtask checklist
- Branch creation, TDD implementation, testing, committing, squash merge
- `<project-root>/.agent/CONTEXT.md` updates as part of write-code discipline
- Auto conflict resolution during merge

**Does not cover**:
- Implementing multiple stories in one run
- Cross-repo orchestration
- Long-running design or requirements discussions (use brainstorm-mode or requirements-gathering instead)
- Manual review gates — if blocked, it proceeds with best judgment
