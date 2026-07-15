---
name: "solid-ocp"
title: "Open Closed Principle Reviewer"
description: "Reviews whether change should modify the original abstraction or extend it based on consumer impact and completeness."
version: "draft-2-agent-ready"
type: "agent-skill"
use_when: "Use after SRP to decide whether behaviour belongs in existing code or should be introduced through extension/composition."
activation: "Load when the user request matches use_when, or when another SOLID instruction file lists this file as a dependency."
depends_on:
  - "solid-core"
  - "solid-srp"
agent_consumption_notes:
  - "Read the metadata first, then follow the instructions as operating context."
  - "Treat imperative instructions as behaviour requirements for the agent."
  - "Do not describe this file as a third-party capability during normal user responses."
  - "Use the user's requested scope as the boundary for action unless the user explicitly expands it."
---

# Open Closed Principle Reviewer

## Name

solid-ocp

## Description

Reviews whether change should modify the original abstraction or extend it based on consumer impact and completeness.

## When To Use

Use after SRP to decide whether behaviour belongs in existing code or should be introduced through extension/composition.

## Dependencies

solid-core, solid-srp

## Agent Consumption Contract

- Use these instructions as active operating context, not as passive documentation.
- Follow direct instructions without referring to the instruction file in user-facing output.
- Preserve the user's scope and ask before extending work beyond that scope.
- Prefer practical, evidence-based action over theoretical explanation unless the user asks to learn.
- If instructions conflict, prefer the option that reduces bugs, maintenance cost, coupling, and future change fragility.

---

## Purpose

The Open Closed Principle (OCP) is not about avoiding modification.

It is about managing change safely.

OCP exists to ensure that new behaviour can be introduced without causing unnecessary impact to unrelated consumers.

This principle applies at every level:

- Function
- Class
- Module
- Service
- Package
- API
- System

---

## Core Philosophy

Most developers misunderstand OCP as:

```text
Never modify existing code.
Always create new code.
```

This is incorrect.

A thing should be modified when the change belongs to its existing responsibility.

A thing should be extended when the new behaviour exists outside its original responsibility.

---

## Change Management Perspective

OCP is fundamentally a change-management principle.

Ask:

```text
Who actually needs this change?
```

If all consumers need the change:

```text
Modify the original.
```

If only a subset of consumers need the change:

```text
Extend the original.
```

This is the primary OCP diagnostic.

---

## Completeness Test

A thing should only be considered closed when it completely expresses its responsibility.

Ask:

```text
Is this thing complete within its current purpose?
```

Examples:

Bird:

```text
Fly
Walk
Peck
Tweet
```

If these behaviours genuinely describe all birds, Bird may be considered complete.

If a Penguin needs additional behaviour:

```text
Swim
```

That behaviour should be added through extension.

---

## Modify vs Extend Heuristic

### Modify When

All consumers require the behaviour.

Examples:

- missing core behaviour
- incorrect behaviour
- incomplete implementation
- defect fixes
- changes required universally

Question:

```text
Would every consumer benefit from this change?
```

If yes:

```text
Modify.
```

---

### Extend When

Only a subset of consumers require the behaviour.

Examples:

- edge cases
- specialisations
- optional features
- additional capabilities

Question:

```text
Would only some consumers require this behaviour?
```

If yes:

```text
Extend.
```

---

## OCP Anti-Patterns

### Anti-Pattern 1: Not Applying OCP

Most OCP failures are simple.

A developer:

1. Makes a change.
2. Breaks a consumer.
3. Changes the consumer.
4. Breaks another consumer.
5. Repeats until the build passes.

No design occurred.

No extension point was considered.

---

### Anti-Pattern 2: Inheritance Chain Hell

Example:

```text
BaseThing
 -> ThingA
   -> ThingB
     -> ThingC
       -> ThingD
```

Large inheritance chains usually indicate that the original abstraction was never properly completed.

---

### Anti-Pattern 3: Premature Closure

Declaring something complete before it properly describes its responsibility.

Example:

```text
BirdWithBeak
BirdWithWings
BirdWithLegs
BirdWithEyes
```

These are not extensions.

They are evidence that Bird was never properly modelled.

---

### Anti-Pattern 4: Speculative Extensibility

When challenged, developers say:

```text
We made it extensible just in case.
```

Apply YAGNI.

Ask:

```text
What actual extension are you expecting?
```

If no credible answer exists:

Investigate whether unnecessary complexity has been introduced.

---

### Anti-Pattern 5: Inheritance Abuse

Look for:

- dependency diamonds
- unnecessary hierarchy depth
- special case inheritance
- inheritance used instead of decomposition

Inheritance should simplify understanding.

If inheritance increases complexity, re-evaluate the design.

---

## Composition vs Extension

OCP does not require inheritance.

Valid extension mechanisms include:

- inheritance
- composition
- higher order components
- decorators
- functional pipelines
- adapters
- strategy implementations

The mechanism is not important.

The change-management outcome is.

---

## Functional Programming Perspective

In functional systems, OCP often appears as composition.

Rather than modifying an existing function:

```text
input
 -> function A
 -> function B
 -> function C
```

Additional behaviour can be introduced by composing new behaviour around existing behaviour.

Avoid creating endless near-identical functions where a compositional approach would suffice.

---

## React Guidance

A common extension mechanism is:

```text
Higher Order Components
```

or equivalent compositional patterns.

Be suspicious when many components receive identical modifications instead of being enhanced through a common extension point.

---

## TypeScript and Node Guidance

Common smells include:

### Function Growth

```text
add
addWithX
addWithY
addWithXY
addWithXYZ
```

Investigate whether a better extension mechanism exists.

---

### Consumer Cascade Changes

If one change causes:

```text
Consumer A
Consumer B
Consumer C
Consumer D
```

to all require modification:

Ask whether the original change belonged inside the existing abstraction.

---

## Design Guidance

Do not design directly for OCP.

Instead:

- decompose correctly
- identify responsibilities
- identify boundaries
- model concepts accurately

Good decomposition naturally creates safer extension points.

OCP should emerge from good design.

---

## Review Questions

Ask:

```text
Is this abstraction complete?
```

```text
Who actually needs this change?
```

```text
Should this modify or extend?
```

```text
What consumers are affected?
```

```text
Can new behaviour be introduced without damaging unrelated consumers?
```

---

## AI Generated Code Review

When reviewing AI-generated code:

1. Identify the original responsibility.
2. Identify the requested change.
3. Determine whether all consumers need the change.
4. Determine whether the abstraction is complete.
5. Decide whether modification or extension is appropriate.

Never assume AI-generated abstractions are correctly extensible.

---

## Success Criteria

OCP is being applied correctly when:

- new behaviour can be introduced safely
- unrelated consumers do not require modification
- completion of existing responsibilities is not mistaken for extension
- abstraction depth remains understandable
- extension points solve real variation
- change impact remains localised

The ultimate goal is simple:

```text
Add new behaviour.
Avoid unnecessary breakage.
Manage change deliberately.
```
