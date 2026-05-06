---
name: write-code
description: Implement code changes with strict TDD (RED→GREEN→REFACTOR), enforcing `coding-standards` and maintaining `<project-root>/.agent/CONTEXT.md`.
---

# Write Code

All coding standards are defined by the [`coding-standards`](../coding-standards/SKILL.md) skill. Before writing code, read it. It covers: deep modules, orthogonality, coupling, seams, adapters, DRY, naming, design-by-contract, testing at interfaces, and refactoring discipline.

## Pre-flight

1. Read `<project-root>/.agent/CONTEXT.md`. If missing, create it following `context-definition` skill discipline. No code without it.
2. Read applicable ADRs in `docs/adr/` (if any exist).
3. Identify which modules, seams, and adapters are involved.
4. Read [`coding-standards`](../coding-standards/SKILL.md) — enforce every standard.

**Hard rule**: No code before red test. Every change follows RED → GREEN → REFACTOR.

## Workflow

### 1. Plan
1. Confirm interface changes with user.
2. Identify which behaviors to test (prioritize).
3. Apply [`coding-standards`](../coding-standards/SKILL.md): identify seam, define interface, verify depth with deletion test.
4. Get user approval.

### 2. Write one test (RED)
- Test at the interface (the seam) — per [`coding-standards`](../coding-standards/SKILL.md) §8.
- Describe behavior, not implementation.
- Use public interface only.
- Expect failure.

### 3. Write minimal code (GREEN)
- Only enough code to pass current test.
- Build deep modules — per [`coding-standards`](../coding-standards/SKILL.md) §1.
- Inject dependencies at seams.
- Validate inputs at seams — per [`coding-standards`](../coding-standards/SKILL.md) §6.
- No speculative code.

### 4. Repeat steps 2–3 for next behavioral slice

### 5. Refactor
- Apply [`coding-standards`](../coding-standards/SKILL.md) §4 (DRY), §2 (orthogonality), §7 (broken windows).
- Deepen modules — consult [deepening](../coding-standards/deepening.md).
- Replace old tests with tests at the new interface — per [`coding-standards`](../coding-standards/SKILL.md) §8.
- Mock only at system boundaries.

### 6. Update `<project-root>/.agent/CONTEXT.md`
- New domain term coined? Add to `<project-root>/.agent/CONTEXT.md` now — same discipline as `context-definition` skill.
- Fuzzy term sharpened during implementation? Update its definition.
- New module-seam relationship? Record it.

## Cases

**Covers**: Any code change — new features, bug fixes, refactors, API surface changes, infrastructure, scripts. If it touches code, this skill handles it.

**Does not cover**: Pure test additions without behavior change, documentation updates without code, configuration-only changes.
