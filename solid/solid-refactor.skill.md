---
name: "solid-refactor"
title: "SOLID Refactor Playbook"
description: "Provides pragmatic, scope-controlled refactoring guidance to improve SOLID compliance without rewriting the world."
version: "draft-2-agent-ready"
type: "agent-skill"
use_when: "Use when a review finds a SOLID problem and the user needs the smallest safe improvement or refactor plan."
activation: "Load when the user request matches use_when, or when another SOLID instruction file lists this file as a dependency."
depends_on:
  - "solid-core"
  - "solid-review"
agent_consumption_notes:
  - "Read the metadata first, then follow the instructions as operating context."
  - "Treat imperative instructions as behaviour requirements for the agent."
  - "Do not describe this file as a third-party capability during normal user responses."
  - "Use the user's requested scope as the boundary for action unless the user explicitly expands it."
---

# SOLID Refactor Playbook

## Name

solid-refactor

## Description

Provides pragmatic, scope-controlled refactoring guidance to improve SOLID compliance without rewriting the world.

## When To Use

Use when a review finds a SOLID problem and the user needs the smallest safe improvement or refactor plan.

## Dependencies

solid-core, solid-review

## Agent Consumption Contract

- Use these instructions as active operating context, not as passive documentation.
- Follow direct instructions without referring to the instruction file in user-facing output.
- Preserve the user's scope and ask before extending work beyond that scope.
- Prefer practical, evidence-based action over theoretical explanation unless the user asks to learn.
- If instructions conflict, prefer the option that reduces bugs, maintenance cost, coupling, and future change fragility.

---

## Purpose

Apply pragmatic SOLID refactoring to improve code safely without rewriting the world.

A good SOLID refactor is not about making code perfect.

It is about making code more open and ready for future change and future refactoring, while reducing the risk that future changes break related or downstream code.

Use this guidance after a SOLID review identifies a design problem, or when generating code that needs to be brought closer to SOLID compliance before being presented.

---

## Core Philosophy

Refactoring exists to make future change safer.

Do not refactor for appearance.

Do not refactor for architectural theatre.

Do not refactor to show cleverness.

Refactor to:

- reduce bugs
- reduce maintenance cost
- reduce coupling
- reduce change blast radius
- improve testability
- improve readability
- make future refactoring easier
- help the code better follow SOLID principles

The best refactor is the smallest change that improves the design enough for the user's requested scope.

---

## Prime Directive

Apply logical and pragmatic patterns.

Make the smallest effective change to the codebase while bringing the code closer to SOLID compliance.

Stay inside the user's requested scope.

Stop before crossing the user's scope boundary unless the user explicitly requested broader refactoring.

---

## Refactor Order

Apply SOLID in principle order:

1. SRP - clarify responsibility first
2. OCP - decide whether to modify or extend
3. LSP - protect substitutability and trust
4. ISP - narrow or reshape contracts
5. DIP - invert concrete dependencies where useful
6. Tests - update and refactor tests after the production refactor

Tests come after the production refactor because the test shape often needs to follow the improved design.

However, do not ignore existing tests. Use them as safety indicators before changing code, and update them after each meaningful refactor step.

---

## Refactor Trigger

Refactor when the current design:

- makes a change affect more than it should
- mixes responsibilities
- requires multiple edits for one requested change
- makes tests harder than necessary
- tightly couples consumers to implementation details
- forces consumers to know too much
- hides side effects
- makes future variation expensive
- duplicates behaviour that should be reusable
- creates fragile or misleading abstractions

Do not refactor simply because code could be prettier.

---

## Smallest Useful Refactor

All small refactors that improve design are worth considering.

The real question is not:

```text
Is this refactor too small?
```

The real question is:

```text
When does this refactor become too big for the user's requested scope?
```

A small useful refactor:

- reduces future maintenance
- improves resilience against change
- removes places bugs can hide
- clarifies one responsibility
- simplifies tests
- reduces coupling
- does not require broad system changes
- stays inside the requested work

Examples:

- extract one unrelated function
- move one side effect behind a clearer boundary
- introduce one narrow interface
- replace one concrete dependency with a contract
- split one bloated method into coherent steps
- remove one duplicated semantic concept

---

## When To Check With The User

Check with the user before continuing when the refactor approaches or crosses the user's scope boundary.

Always check before refactoring:

- a whole class when the user only asked for a small change
- multiple files outside the requested area
- public interfaces used by other teams
- shared libraries or packages
- API contracts
- database schemas
- architecture-level flows
- legacy code relied on by many consumers
- behaviour that could affect existing users

If the user explicitly requested a wider refactor, proceed within that stated scope.

---

## Stop Conditions

Stop refactoring when:

- the requested change is complete
- the code is solid enough for the requested scope
- further improvement would cross the user's scope boundary
- further improvement would require architectural approval
- further improvement would touch unrelated consumers
- the next change belongs in a separate ticket
- a clean stopping point has been reached

If the code still has wider issues, expose those as observations or technical debt rather than continuing unauthorised work.

If the user leaves the request open-ended, define the boundary around the change being made and stop at the cleanest safe point.

If the refactor genuinely needs to spread, stop at a clean point and ask for permission to continue.

---

## Hard No Rules

Never refactor in a way that:

- accidentally changes behaviour
- breaks public contracts
- breaks downstream consumers
- makes tests worse
- hides design debt
- renames everything for style or vibes
- creates new code when existing code should be refactored for reuse
- increases coupling
- makes one change require multiple future changes
- makes the code harder to test
- makes the code harder to reason about
- introduces abstractions with no substitution value
- changes code outside the user's requested sphere of influence without permission

Do not create new parallel code when the correct fix is to improve the existing reusable code.

---

## Behaviour Preservation

Refactoring should preserve externally visible behaviour unless the user explicitly requested behaviour change.

Before changing code, identify:

- expected inputs
- expected outputs
- side effects
- error behaviour
- public contracts
- existing tests
- downstream consumers

After changing code, verify that external behaviour is still equivalent unless the requested change says otherwise.

---

## Test Handling

Update tests after each meaningful refactor.

Expect tests to need refactoring too.

Tests should also become more SOLID as the production code becomes more SOLID.

Good SOLID refactoring usually makes tests:

- simpler
- more focused
- easier to read
- easier to isolate
- easier to mock or fake
- higher coverage with less technical effort
- less dependent on unrelated setup

Do not treat test refactoring as optional clean-up if the production design has changed.

---

## Test Refactor Rules

When refactoring tests:

- keep tests aligned to behaviour
- remove duplicated setup where it hides intent
- split tests that verify unrelated behaviours
- update mocks/fakes to match new contracts
- avoid testing implementation details accidentally exposed by bad design
- preserve meaningful coverage
- avoid coverage-only tests that assert existence without behaviour

If a refactor makes tests harder, investigate whether the refactor moved in the wrong direction.

---

## Practical Refactor Toolbox

Use pragmatic judgement rather than pattern worship.

Useful moves include:

### Extract Function

Use when a block of logic has a separate responsibility or a nameable step.

### Extract Class

Use when a group of methods/data has its own reason to change.

### Extract Interface

Use when consumers need a stable contract or implementations need to be swappable.

### Split Interface

Use when consumers depend on methods or data they do not need.

### Introduce Adapter

Use when an external or incompatible implementation needs to fit a stable internal contract.

### Introduce Strategy

Use when behaviour varies behind a stable decision point.

### Move Side Effect

Use when a unit does useful calculation but also performs unrelated mutation, logging, persistence, publishing, or external calls.

### Wrap Concrete Dependency

Use when high-level code depends directly on infrastructure, SDKs, packages, or volatile concrete implementations.

### Move Construction To Boundary

Use when business logic creates dependencies directly.

### Consolidate Duplicated Contract

Use when multiple interfaces or DTOs express the same semantic concept under different names.

### Replace Special Case With Capability

Use when a subtype or consumer branches because a base abstraction has the wrong shape.

Do not apply patterns because they are available.

Apply a pattern only when it reduces bugs, maintenance, coupling, or future change risk.

---

## Refactor Quality Heuristic

A refactor is good when it answers yes to these questions:

```text
Is the result more maintainable?
```

```text
Is the result more resilient against change?
```

```text
Does the result reduce places for bugs to hide?
```

If the answer is no, reconsider the refactor.

---

## Bad Refactor Smells

A refactor has probably made the design worse when:

- code is harder to test
- tests need more setup than before
- coupling increases
- one future change now requires multiple edits
- abstractions are less honest
- responsibilities are more blurred
- new code duplicates reusable existing behaviour
- consumers know more than before
- concrete dependencies spread further
- interfaces become larger without clear reason
- tiny fragments become impossible to compose
- readability drops
- behaviour becomes harder to trace

If a refactor moves opposite to maintainability, resilience, and bug reduction, stop and reassess.

---

## Legacy Code Guidance

Proceed with extreme caution around legacy code.

Legacy code may be ugly and still relied on by many parts of the system.

Before touching legacy code:

1. Identify consumers.
2. Identify existing behaviour.
3. Identify tests or missing tests.
4. Identify the change boundary.
5. Make a plan.
6. Discuss the plan with the user when the impact is broad.

Do not casually refactor legacy code that many consumers depend on.

It may be safer to first create a plan for a larger update that converts the legacy area towards SOLID from top to bottom.

Assist the user in architecting that plan when requested.

---

## Legacy Incremental Strategy

When legacy code cannot be fully fixed now:

- add characterisation tests if useful
- isolate one seam
- extract the smallest stable responsibility
- wrap volatile concrete dependencies
- introduce adapters at boundaries
- avoid broad rewrites
- preserve behaviour
- create visible technical debt for remaining work

Prefer a safe seam over a heroic rewrite.

---

## Scope Boundary Handling

When refactoring reveals work outside the requested scope:

1. Stop at a clean boundary.
2. Explain the wider issue briefly.
3. State why continuing would exceed the current scope.
4. Offer a small technical debt item or follow-up plan.
5. Ask before proceeding further.

Do not silently expand the scope.

Do not ignore the issue either.

---

## AI-Generated Code Refactoring

When the agent generates code:

1. Generate the requested code.
2. Run the SOLID review silently when the change is non-trivial.
3. Apply in-scope refactors silently.
4. Refactor tests after production code when relevant.
5. Present the improved answer.

If the SOLID-compliant refactor requires changing code outside the requested scope, ask first.

Do not knowingly present first-pass AI code that is fragile to change.

---

## Relationship With SOLID Principles

### SRP

Refactor responsibilities first.

If the code cannot be described without a subject-changing "and", split or relocate the extra responsibility.

### OCP

Decide whether the existing code should be modified or extended.

Modify when all consumers need the change.

Extend when only some consumers need it.

### LSP

Protect substitutability.

Do not refactor into a shape where consumers must know which implementation they received.

### ISP

Make consumers depend only on useful, coherent contracts.

Do not split interfaces into dust or merge everything into a dumping ground.

### DIP

Move concrete decisions out of high-level code where change, tests, or substitution justify it.

Do not inject every tiny internal helper automatically.

---

## Technical Debt Handling

If a refactor should happen but cannot happen within the current scope, create or recommend technical debt.

Include:

- what should be refactored
- why it matters
- what risk remains
- what the small safe path looks like
- what acceptance criteria would prove the debt is resolved

Do not use technical debt as a way to hide poor design.

Use it to keep necessary future work visible.

---

## Refactor Output Template

Use this structure when giving a refactor plan.

```markdown
## Refactor Goal
<What design risk is being reduced.>

## Current Problem
<What is wrong and which SOLID principles are involved.>

## Scope Boundary
<What will and will not be changed.>

## Smallest Safe Refactor
<The smallest effective change.>

## Steps
1. <step>
2. <step>
3. <step>

## Test Updates
<How tests should change after the refactor.>

## Risks
<What could break and how to protect against it.>

## Follow-up Debt
<Any remaining out-of-scope work.>
```

---

## Do Not

Do not refactor endlessly.

Do not chase perfection.

Do not cross the user's scope boundary without permission.

Do not replace understandable code with clever abstractions.

Do not make code harder to test.

Do not make downstream consumers pay for local design choices.

Do not create new near-duplicate code when reusable existing code should be improved.

Do not use SOLID as an excuse for a rewrite unless the user explicitly asks for one.

---

## Success Criteria

The refactor is successful when:

- the requested change is complete
- behaviour is preserved unless intentionally changed
- the code is closer to SOLID compliance
- future change is easier
- downstream consumers are protected
- tests are simpler or more meaningful
- coupling is reduced or contained
- bug surface area is reduced
- the refactor stays within scope
- wider issues are captured as visible debt rather than hidden

---

## Core Summary

```text
A good SOLID refactor makes future change safer.
```

```text
Refactor in SOLID order, then update tests.
```

```text
The question is not whether a refactor is too small; the question is when it becomes too big for the user's scope.
```

```text
Stop when the requested scope is complete or the next improvement crosses the user's boundary.
```

```text
Make the smallest effective change that improves maintainability, change resilience, and bug reduction.
```
