---
name: coding-standards
description: Enforce coding standards! deep interfaces, orthogonal design, seams, DRY, deliberate naming, design-by-contract, testability. Use when writing code, reviewing code, or refactoring to ensure good architecture.
---

# Coding Standards

Code according to these standards. Use the vocabulary in [language.md](language.md) — **module**, **interface**, **depth**, **seam**, **adapter**, **leverage**, **locality** — exactly. See [deepening.md](deepening.md) for deepening patterns and [interface-design.md](interface-design.md) for interface exploration.

## Core standards

### 1. Deep, not shallow

Make every module **deep** — high leverage behind a simple interface. Before writing code:

- What behaviour does the caller need? Minimise the interface to that.
- Run the **deletion test**: if you delete the module, does complexity vanish (pass-through) or reappear at N callers (earning its keep)?
- **The interface is the test surface.** Tests cross the same seam as callers.

### 2. Orthogonality

Changes should be independent. Two concerns are orthogonal when modifying one requires no changes to the other.

- Isolate concepts so they change independently. If a single requirement change touches many files, orthogonality is broken.
- Separate _what_ from _how_ (validation from transformation from persistence).
- One responsibility per module. If you can't name it in one concept, it holds two.

### 3. Minimise coupling

Coupling is the enemy of orthogonality. Reduce it by:

- Putting **seams** where behaviour varies. Inject dependencies through seams.
- **One adapter = hypothetical seam. Two adapters = real seam.** Don't introduce a port unless something actually varies across it.
- Keep internal seams private. Don't expose implementation details through the interface.

### 4. DRY

Each piece of knowledge must have one representation. Duplication breeds inconsistency.

- Extract when the same logic, data shape, or invariant appears in two places.
- Prefer extraction that increases **locality** (change concentrates in one place).
- Don't extract just for extraction's sake — apply the deletion test.

### 5. Program deliberately

Every name and structure should carry meaning.

- Name things for the domain concept they represent, not the mechanism.
- If a name needs explanation, rename it. The name is the first and often only documentation.
- Don't assume — prove with tests.

### 6. Design by contract

Make invariants explicit. Fail fast.

- Validate inputs at seams. Bad data should not propagate.
- Use assertions for internal invariants that should never be violated.
- Document ordering, performance, and error modes as part of the interface.

### 7. Broken windows

Never ship sloppy code. Fix structural problems immediately or they multiply — refactor before building on messy foundations.

### 8. Ruthless testing

Code is only done when tests pass.

- Test at the **interface** (the seam). Tests survive internal refactors.
- When deepening replaces shallow modules, delete old tests on the old modules and write new tests at the new interface.
- Tests describe behaviour, not implementation.

## Workflows

### Writing new code

1. Read [language.md](language.md) terminology. Read `CONTEXT.md` for domain vocabulary.
2. Identify the seam — where does a dependency vary?
3. Define the interface (types, invariants, error modes, ordering).
4. Verify depth: does the interface hide meaningful behaviour? Deletion test.
5. Implement. Inject dependencies at seams.
6. Test at the interface.

### Refactoring

1. Find friction — where does understanding one concept require bouncing through many modules?
2. Classify dependencies (see [deepening.md](deepening.md)).
3. Deepen the cluster: merge shallow modules, place the seam, inject adapters.
4. Replace old tests with tests at the new interface.

### Reviewing code

- Is the module deep? Or is the interface as complex as the implementation?
- Are concerns orthogonal? Or does one change require touching unrelated modules?
- Is coupling minimised? Are seams placed where things vary?
- Is anything duplicated that could be one source of truth?
- Does every name carry domain meaning?
- Are invariants enforced at the seams?
- Do tests sit at the interface, or are they leaking into implementation details?
