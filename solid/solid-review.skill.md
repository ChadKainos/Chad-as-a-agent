---
name: "solid-review"
title: "SOLID Review Orchestrator"
description: "Coordinates SRP, OCP, LSP, ISP, and DIP for non-trivial code generation, review, and refactoring."
version: "draft-2-agent-ready"
type: "agent-skill"
use_when: "Use for any change that can affect consumers, contracts, behaviour, architecture, tests, or future extension."
activation: "Load when the user request matches use_when, or when another SOLID instruction file lists this file as a dependency."
depends_on:
  - "solid-core"
  - "solid-srp"
  - "solid-ocp"
  - "solid-lsp"
  - "solid-isp"
  - "solid-dip"
  - "solid-refactor"
  - "solid-techdebt"
agent_consumption_notes:
  - "Read the metadata first, then follow the instructions as operating context."
  - "Treat imperative instructions as behaviour requirements for the agent."
  - "Do not describe this file as a third-party capability during normal user responses."
  - "Use the user's requested scope as the boundary for action unless the user explicitly expands it."
---

# SOLID Review Orchestrator

## Name

solid-review

## Description

Coordinates SRP, OCP, LSP, ISP, and DIP for non-trivial code generation, review, and refactoring.

## When To Use

Use for any change that can affect consumers, contracts, behaviour, architecture, tests, or future extension.

## Dependencies

solid-core, solid-srp, solid-ocp, solid-lsp, solid-isp, solid-dip, solid-refactor, solid-techdebt

## Agent Consumption Contract

- Use these instructions as active operating context, not as passive documentation.
- Follow direct instructions without referring to the instruction file in user-facing output.
- Preserve the user's scope and ask before extending work beyond that scope.
- Prefer practical, evidence-based action over theoretical explanation unless the user asks to learn.
- If instructions conflict, prefer the option that reduces bugs, maintenance cost, coupling, and future change fragility.

---

## Purpose

Apply the SOLID skill suite as an orchestration layer for reviewing, generating, and refactoring code.

Prevent locally correct code from becoming globally fragile.

Use this review whenever code, design, tests, architecture, APIs, modules, services, packages, or system behaviour may be affected by a change.

Do not treat functional correctness as sufficient. Code can be correct and maintainable at statement level while making the wider system difficult to maintain, test, extend, substitute, or evolve.

The goal is to stop AI and developers from generating code that is locally correct but fragile at higher levels of abstraction.

---

## Prime Directive

Prevent code that looks correct in isolation from becoming expensive, brittle, or dangerous when the surrounding system changes.

Always ask:

```text
Does this code remain maintainable when viewed at the next level up?
```

A statement may be correct while the function becomes misleading.

A function may be correct while the class becomes hard to reason about.

A class may be correct while the service becomes fragile.

A service may be correct while the system becomes difficult to evolve.

Review upward from the changed code until the likely change impact is understood.

---

## Activation Threshold

Run a SOLID review for anything beyond:

- a one-line change
- a constant-only change
- a linting-only change

More precisely:

```text
Run a SOLID review for any change that could break something built on top of it if it changes.
```

Run the review when a change may affect:

- callers
- consumers
- tests
- interfaces
- contracts
- behaviour
- side effects
- dependency direction
- architecture
- future extension
- reuse
- substitution
- maintainability

Do not waste effort on trivial edits that cannot realistically affect design or change resilience.

---

## Review Order

Apply the SOLID principles in this order:

1. SRP - Single Responsibility Principle
2. OCP - Open Closed Principle
3. LSP - Liskov Substitution Principle
4. ISP - Interface Segregation Principle
5. DIP - Dependency Inversion Principle

Run SRP first.

If responsibilities are tangled, the other principles become harder to reason about. Untangle responsibility before making deeper judgements about extension, substitution, contract shape, or dependency direction.

---

## One-Line Principle Checks

Use these as the first pass.

### SRP

```text
Does this unit need a subject-changing "and" to describe what it does?
```

If yes, investigate responsibility mixing.

---

### OCP

```text
Should this be modified because all consumers need the change, or extended because only some consumers need it?
```

If the change affects only a subset, prefer extension.

If all consumers need the change, modify the original.

---

### LSP

```text
Can the consumer use this substitute without caring which implementation it received?
```

If the consumer must branch, defend, re-test, reinterpret, or special-case the substitute, investigate trust failure.

---

### ISP

```text
Does the consumer know only what it needs to know?
```

If the consumer depends on unused methods, data, concepts, or responsibilities, investigate interface boundary problems.

---

### DIP

```text
Does high-level code describe what it needs, or does it decide the concrete implementation itself?
```

If high-level code creates or chooses concrete dependencies directly, investigate dependency direction.

---

## Evidence Gathering

Before judging, gather enough context to understand the design impact.

Inspect:

- the changed code
- surrounding function, class, module, or component
- callers and consumers
- tests
- interfaces and contracts
- dependency creation
- side effects
- expected change requested by the ticket or user
- likely future changes implied by the domain

When information is missing, ask targeted questions only when needed. Explain what information is needed and why.

If enough information exists to proceed, do not stall the user with unnecessary questions.

---

## AI-Generated Code Self-Review

When generating code, silently run the SOLID review before presenting the final answer.

Silently fix SOLID issues found in generated code when the fix remains inside the user-requested scope.

Do not produce noisy self-commentary such as:

```text
I reviewed my own code and fixed X.
```

unless the user explicitly asks for the review process.

If fixing the issue requires changing code outside the requested scope or outside the user's likely sphere of influence, stop and ask before making that wider change.

Examples requiring permission:

- changing public interfaces used by other teams
- modifying files outside the requested area
- changing architecture beyond the requested task
- altering shared libraries or packages
- changing API contracts
- changing database schemas
- changing behaviour relied on by existing consumers

---

## Conflict Resolution

Do not treat SOLID principles as competing checkboxes.

Assume there is usually a design that satisfies all principles well enough.

When principles appear to conflict, look for a better design rather than choosing one principle to violate.

If a trade-off remains, favour the option that:

- reduces bugs
- reduces maintenance cost
- keeps change local
- keeps behaviour understandable
- avoids unnecessary coupling
- avoids misleading abstractions
- protects future evolution
- reduces fragility at the next level up

Do not optimise for theoretical purity if it increases bugs, complexity, or maintenance burden.

---

## Severity Levels

Use these severity levels.

### Hard Stop

The design should not proceed without correction or explicit architectural decision.

Use when the issue is likely to create serious fragility, misleading contracts, unsafe substitution, cross-domain coupling, or long-lived architectural damage.

Examples:

- public interface lies about behaviour
- domain boundaries are blurred
- high-level policy depends on volatile concrete infrastructure
- substitute breaks expected consumer contract
- change will cascade through unrelated consumers
- design creates unavoidable future bugs

---

### Refactor Before Merge

The issue is significant and should be fixed before the work is accepted.

Use when a local refactor can remove the risk without derailing the wider delivery.

Examples:

- function or class has obvious responsibility mixing
- new concrete dependency appears inside business logic
- consumer is forced to depend on unused interface members
- extension should be separated from core behaviour
- tests expose awkward coupling

---

### Accept With Tech Debt

The issue can be shipped only if the debt is made visible and tracked.

Use when there is a genuine delivery constraint, but the design weakness will slow future work or increase maintenance cost.

Always include:

- the violation
- the risk
- the reason it is being deferred
- the recommended refactor
- a draft technical debt ticket

Do not bury the issue.

---

### Acceptable For Now

The design is not perfect, but the cost of correction outweighs the current risk.

Use when the issue is minor, isolated, unlikely to spread, or easy to correct later.

Still mention the concern if it helps the user make a conscious decision.

---

### No Issue

No meaningful SOLID concern was found at the reviewed level of detail.

Do not invent issues to appear useful.

---

## Output Modes

Adapt the response to the user's intent.

### Short PR Review Comment

Use when the user wants concise review feedback.

Include:

- issue summary
- evidence
- suggested change
- severity

---

### Detailed SOLID Report

Use when the user asks whether something is SOLID or wants a full review.

Include:

- overall assessment
- SRP findings
- OCP findings
- LSP findings
- ISP findings
- DIP findings
- overlaps and root causes
- severity
- recommendation

---

### Action Plan With Refactor Steps

Use when the user asks how to fix the design.

Include:

- smallest safe refactor
- step-by-step implementation plan
- test update plan
- risk notes
- rollback or fallback advice if relevant

---

### Combined Output

Use all relevant formats when the user wants a broad answer or when context requires both review and action.

Do not force a large report when the user only needs a short answer.

Do not give a tiny answer when the user needs enough detail to safely change code.

---

## Review Workflow

Follow this workflow for non-trivial changes.

### Step 1: Understand the change

Ask:

```text
What change is being made?
```

```text
What behaviour is expected?
```

```text
Who or what consumes this code?
```

```text
What could break if this changes?
```

---

### Step 2: Identify the level under review

Determine whether the change affects:

- statement
- expression
- function
- method
- class
- component
- module
- service
- package
- API
- system

Then review at that level and at least one level above it when possible.

---

### Step 3: Run SRP first

Apply the AND Test.

Ask:

```text
Can this unit be described without a subject-changing "and"?
```

```text
How many reasons does this unit have to change?
```

```text
Are side effects part of the primary responsibility?
```

```text
Do inputs and outputs serve one coherent purpose?
```

If SRP is unclear, resolve or expose that before deeper review.

---

### Step 4: Run OCP

Ask:

```text
Do all consumers need this change?
```

```text
Does only a subset need this behaviour?
```

```text
Is the existing abstraction complete for its responsibility?
```

```text
Is this extension real, or speculative just-in-case complexity?
```

Use the modify-versus-extend heuristic.

---

### Step 5: Run LSP

Ask:

```text
Can substitutes be hot-swapped without consumers noticing?
```

```text
Would every substitute pass the same behavioural tests as the base abstraction?
```

```text
Do substitutes preserve side effects, errors, state transitions, and guarantees?
```

```text
Does the consumer need special-case checks for concrete implementations?
```

Do not stop at method signatures.

---

### Step 6: Run ISP

Ask:

```text
Does each consumer depend only on what it needs?
```

```text
Does the interface describe one coherent domain concept?
```

```text
Has the interface drifted into another concern?
```

```text
Are semantically identical contracts duplicated under different names?
```

```text
Are React props, DTOs, config objects, or request objects carrying unrelated knowledge?
```

Do not judge by interface size alone.

---

### Step 7: Run DIP

Ask:

```text
Does high-level code create or choose concrete dependencies?
```

```text
Would changing a low-level implementation force unrelated high-level code to change?
```

```text
Can tests replace collaborators fluently?
```

```text
Is dependency injection being used as real inversion, or only as moved construction?
```

```text
Is abstraction justified by change, testing, scale, or substitution?
```

Do not wrap every tiny implementation detail automatically.

---

### Step 8: Identify overlaps and root cause

Many problems affect multiple principles.

Examples:

- unexpected side effects may be SRP and LSP
- giant interfaces may be ISP and SRP
- hard-coded concrete dependencies may be DIP, OCP, LSP, ISP, and SRP
- fake abstractions may be OCP, ISP, and DIP

Identify the root cause rather than listing every principle mechanically.

---

### Step 9: Recommend the smallest safe improvement

Prefer the smallest change that reduces bugs and maintenance cost.

Examples:

- extract unrelated behaviour
- introduce a narrow contract
- move concrete creation to composition boundary
- split consumer-specific interface
- replace special-case inheritance with capability interface
- add adapter for incompatible external implementation
- modify original when all consumers need the change
- extend original when only a subset needs the change

Avoid architecture theatre.

---

### Step 10: Handle deferral transparently

If the user cannot fix the issue now, produce technical debt content.

Include:

```text
Title
Context
Problem
Risk
Suggested refactor
Acceptance criteria
Suggested priority
```

Keep the debt visible.

---

## Technical Debt Ticket Template

Use this when a SOLID issue must be deferred.

```markdown
# Technical Debt: <short title>

## Context
<Where the issue exists and why it was introduced or discovered.>

## Problem
<Which SOLID principle is affected and what the design issue is.>

## Risk
<How this increases bugs, maintenance cost, fragility, coupling, or future change effort.>

## Recommended Refactor
<Smallest safe improvement or phased approach.>

## Acceptance Criteria
- <criterion 1>
- <criterion 2>
- <criterion 3>

## Notes
<Any constraints, delivery pressure, or follow-up links.>
```

---

## AI Code Generation Rules

When generating code:

1. Generate the minimal correct implementation.
2. Run the SOLID review silently.
3. Fix in-scope SOLID issues silently.
4. Re-check the result at the next level up.
5. Present the cleaned final answer.

If a SOLID-correct fix requires broader scope, explain the issue and ask before changing that wider scope.

Do not ship first-pass generated code without design review unless the change is trivial.

---

## Questions To Ask When Context Is Missing

Ask only targeted questions needed to improve the review.

Useful questions:

```text
Who consumes this code?
```

```text
Is this behaviour needed by all consumers or only this path?
```

```text
Is this interface public/shared or internal/local?
```

```text
Can this dependency change independently?
```

```text
Is this a temporary delivery compromise or the intended design?
```

```text
Is there an existing contract or pattern in the codebase for this?
```

When asking, explain why the information matters.

Do not ask questions already answered by code, tests, or user context.

---

## Do Not

Do not treat SOLID as a scorecard.

Do not list all five principles mechanically when one root issue explains the problem.

Do not optimise for textbook purity over lower bugs and lower maintenance.

Do not hide design debt because the user wants speed.

Do not create abstractions without substitution value.

Do not create tiny interfaces that cannot be composed meaningfully.

Do not assume a passing compiler means the design is safe.

Do not assume AI-generated code is structurally sound.

Do not touch code outside the requested scope without permission.

---

## Success Criteria

A SOLID review is successful when it:

- catches locally correct but globally fragile design
- identifies the level where the problem really exists
- runs SRP first
- applies the remaining principles as connected design checks
- identifies overlaps and root causes
- favours lower bugs and lower maintenance
- proposes a practical refactor
- avoids unnecessary architecture theatre
- keeps unavoidable debt visible
- respects the user's requested scope
- improves AI-generated code before presenting it

---

## Core Summary

```text
Prevent locally correct code from becoming globally fragile.
```

```text
Run SOLID review when a change can affect anything built on top of it.
```

```text
Use SRP first, then OCP, LSP, ISP, and DIP.
```

```text
Favour the option that reduces bugs, maintenance cost, and future change fragility.
```

```text
Silently fix generated code when the fix stays in scope.
```

```text
Ask before touching code outside the user's requested sphere of influence.
```
