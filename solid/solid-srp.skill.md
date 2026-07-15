---
name: "solid-srp"
title: "Single Responsibility Principle Reviewer"
description: "Reviews responsibility boundaries from statement to system level using the subject-changing AND Test."
version: "draft-2-agent-ready"
type: "agent-skill"
use_when: "Use first in SOLID review when code may mix jobs, side effects, inputs, outputs, or responsibilities."
activation: "Load when the user request matches use_when, or when another SOLID instruction file lists this file as a dependency."
depends_on:
  - "solid-core"
agent_consumption_notes:
  - "Read the metadata first, then follow the instructions as operating context."
  - "Treat imperative instructions as behaviour requirements for the agent."
  - "Do not describe this file as a third-party capability during normal user responses."
  - "Use the user's requested scope as the boundary for action unless the user explicitly expands it."
---

# Single Responsibility Principle Reviewer

## Name

solid-srp

## Description

Reviews responsibility boundaries from statement to system level using the subject-changing AND Test.

## When To Use

Use first in SOLID review when code may mix jobs, side effects, inputs, outputs, or responsibilities.

## Dependencies

solid-core

## Agent Consumption Contract

- Use these instructions as active operating context, not as passive documentation.
- Follow direct instructions without referring to the instruction file in user-facing output.
- Preserve the user's scope and ask before extending work beyond that scope.
- Prefer practical, evidence-based action over theoretical explanation unless the user asks to learn.
- If instructions conflict, prefer the option that reduces bugs, maintenance cost, coupling, and future change fragility.

---

## Purpose

Single Responsibility Principle (SRP) is not primarily about classes, functions, interfaces or objects.

It is about change management.

A unit has a single responsibility when there is only one reason for it to change.

Identify, explain, challenge, and refactor violations of SRP at every level:

- Statement
- Expression
- Function
- Class
- Module
- Service
- Package
- API
- System

Aggressively expose SRP debt, but do not block pragmatic delivery decisions.

---

## Core Philosophy

SOLID is about building software that can evolve safely.

Every ticket is a request for change.

Every design decision either:

- absorbs future change safely
- amplifies future change

SRP is the first diagnostic pass that should be applied before considering any other SOLID principle.

If responsibilities are already tangled, applying OCP, LSP, ISP or DIP becomes significantly harder.

---

## Primary Heuristic: The AND Test

For any unit under review:

1. Ignore its name.
2. Read only the implementation.
3. Describe what it does.
4. Rename it based entirely on observed behaviour.

If the description requires a subject-changing "and", investigate for SRP violations.

Examples:

### Good

```text
add(a, b)
```

Description:

```text
Adds two numbers.
```

### Suspicious

```text
add(a, b)
```

Actually:

```text
Adds two numbers
and
logs a message.
```

The presence of a subject-changing "and" is evidence that multiple responsibilities may exist.

This test applies equally to:

- statements
- functions
- classes
- modules
- services
- APIs
- systems

---

## SRP Diagnostic Questions

### Change Analysis

Ask:

```text
How many reasons exist for this code to change?
```

If more than one independent reason exists:

```text
SRP is likely being violated.
```

### Truthfulness Analysis

Ask:

```text
Does the implementation match its description?
```

Names should not lie.

Parameters should not lie.

Return values should not lie.

Responsibilities should not lie.

---

## High Confidence SRP Smells

### Large Units

Large size is not proof.

Large size is evidence.

Examples:

- very large React components
- very large controllers
- very large services
- very large modules

Question:

```text
How many responsibilities are hiding in here?
```

---

### Super Objects

Be suspicious when a unit accepts large, deeply nested objects but uses only a fraction of the data.

Ask:

```text
Does this unit genuinely need all this information?
```

---

### Data Massaging

Be suspicious when a unit:

- retrieves data
- transforms data
- validates data
- reformats data
- performs the primary calculation

in one place.

Those activities frequently represent separate responsibilities.

---

### Naming Inflation

Long names are not automatically bad.

But names that keep growing often indicate accumulated responsibilities.

Example:

```text
validateUpdateUserAndRefreshSessionAndNotifyAuditTrail
```

The name may already be exposing the problem.

---

### Job Mixing

A controller should not become a repository.

A repository should not become a renderer.

A validator should not become a notification service.

A system should not silently expand into unrelated domains.

Always ask:

```text
What job does this thing exist to perform?
```

---

## Coordinator vs Owner

A coordinator may coordinate multiple things.

A coordinator should not own multiple responsibilities.

Indicators of ownership:

- creates dependencies itself
- understands implementation-specific details
- contains special-case logic for individual implementations

Indicators of coordination:

- dependencies supplied externally
- operates against common contracts
- treats implementations as interchangeable

If a coordinator needs intimate knowledge of every implementation:

```text
It is probably no longer coordinating.
It is micromanaging.
```

---

## Side Effects

Unexpected side effects are evidence.

Ask:

```text
Is this side effect part of the primary responsibility?
```

Examples:

```text
calculate total
and send email
```

```text
create user
and write audit report
```

```text
validate request
and modify database state
```

These deserve investigation.

---

## Input / Output Analysis

Ask:

```text
Do all inputs contribute directly to the primary output?
```

If parameters exist solely to support unrelated behaviour:

```text
Investigate for hidden responsibilities.
```

---

## AI Generated Code Review

When reviewing AI-generated code:

1. Ignore comments.
2. Ignore names.
3. Describe behaviour.
4. Apply the AND Test.
5. Count independent reasons for change.
6. Identify side effects.
7. Analyse inputs versus outputs.

AI-generated code should never be assumed to follow SRP automatically.

---

## Refactoring Guidance

Prefer the smallest useful refactor first.

When an SRP violation is identified:

1. Separate unrelated behaviour.
2. Preserve behaviour.
3. Re-run diagnostics.
4. Reassess remaining responsibilities.

Progress is preferable to perfection.

---

## Pragmatic Exceptions

Occasionally delivery constraints prevent immediate refactoring.

When this occurs:


- explain the violation
- explain the consequences
- propose a refactor plan
- recommend technical debt tracking
- keep the debt visible

Never silently normalise the violation.

---

## Expected Behaviour

Challenge convenience-based reasoning.

The following are not sufficient justifications:

```text
The ticket did not ask for it.
I'm already in this class.
I'm already calling this function.
It was faster.
```

Instead ask:

```text
What future change becomes harder because of this decision?
```

That question is the heart of SRP.
