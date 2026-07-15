---
name: "solid-lsp"
title: "Liskov Substitution Principle Reviewer"
description: "Reviews whether substitutes preserve trust, behaviour, contracts, side effects, exceptions, and consumer expectations."
version: "draft-2-agent-ready"
type: "agent-skill"
use_when: "Use when one implementation, test double, adapter, union type, generic, API version, or component claims to stand in for another."
activation: "Load when the user request matches use_when, or when another SOLID instruction file lists this file as a dependency."
depends_on:
  - "solid-core"
  - "solid-srp"
  - "solid-ocp"
agent_consumption_notes:
  - "Read the metadata first, then follow the instructions as operating context."
  - "Treat imperative instructions as behaviour requirements for the agent."
  - "Do not describe this file as a third-party capability during normal user responses."
  - "Use the user's requested scope as the boundary for action unless the user explicitly expands it."
---

# Liskov Substitution Principle Reviewer

## Name

solid-lsp

## Description

Reviews whether substitutes preserve trust, behaviour, contracts, side effects, exceptions, and consumer expectations.

## When To Use

Use when one implementation, test double, adapter, union type, generic, API version, or component claims to stand in for another.

## Dependencies

solid-core, solid-srp, solid-ocp

## Agent Consumption Contract

- Use these instructions as active operating context, not as passive documentation.
- Follow direct instructions without referring to the instruction file in user-facing output.
- Preserve the user's scope and ask before extending work beyond that scope.
- Prefer practical, evidence-based action over theoretical explanation unless the user asks to learn.
- If instructions conflict, prefer the option that reduces bugs, maintenance cost, coupling, and future change fragility.

---

## Purpose

Apply the Liskov Substitution Principle (LSP) as a practical design, review, and refactoring framework.

LSP is not really about inheritance.

It is about trust.

A consumer of an abstraction should not need to know which implementation it has received. If one implementation can be swapped for another and the consuming code behaves correctly without special handling, the substitution is probably valid.

A substitute is valid when it fulfils the same contract, expectations, behaviour, return shape, error behaviour, and side effects as the thing it replaces.

Use LSP to reduce the risk of change when implementations evolve, specialise, derive, compose, or vary over time.

---

## Core Philosophy

Do not treat LSP as a Java or C# inheritance rule only.

Apply it anywhere one thing claims it can stand in for another:

- subclass replacing base class
- implementation replacing interface
- derived interface replacing parent interface
- generic specialisation replacing generic base behaviour
- union type variation replacing previous accepted shape
- composed component replacing another component
- adapter replacing direct implementation
- test double replacing real implementation
- API version replacing previous API version
- module replacing another module

The question is always:

```text
Can this be used wherever the original was expected, without the consumer needing to know or care?
```

If the consumer has to know which implementation it received, trust has broken down.

---

## Plain-English Definition

A child should be able to take its parent's role without anything unexpectedly breaking.

More generally:

```text
A replacement must keep the promises made by the thing it replaces.
```

Those promises include more than method names and types.

They include:

- accepted inputs
- return values
- error behaviour
- side effects
- state transitions
- timing expectations where explicitly part of the contract
- invariants
- consumer assumptions
- domain meaning

Do not stop at shape compatibility. Behavioural compatibility matters more.

---

## Primary Diagnostic: The Hot Swap Test

Use the Hot Swap Test as the main LSP heuristic.

Ask:

```text
Could I secretly swap this implementation for the original while the consumer is away, and would the consumer continue working without noticing?
```

If yes, the substitution is likely valid.

If no, investigate for LSP violations.

A likely LSP violation exists when replacing the original with the substitute forces consuming code to add:

- implementation checks
- special cases
- new error handling
- different sequencing
- different assumptions
- different state management
- different side-effect expectations
- different tests for the inherited/base behaviour

---

## Secondary Diagnostic: Shared Test Suite

A likely LSP violation exists when a derived implementation cannot pass the same behavioural tests as the base abstraction.

For a base type, interface, or abstraction:

```text
BaseBehaviourTests
```

Every substitute should pass those same tests:

```text
ImplementationATests -> includes BaseBehaviourTests
ImplementationBTests -> includes BaseBehaviourTests
ImplementationCTests -> includes BaseBehaviourTests
```

Additional tests are allowed for additional behaviour.

Replacement tests are suspicious.

If the substitute requires a completely different test suite for the shared behaviour, it may not be a valid substitute.

---

## Behaviour Beats Signature

A matching method signature is not enough.

This is not sufficient:

```ts
interface Printer {
  print(): void;
}
```

with:

```ts
class PdfPrinter implements Printer {
  print(): void {}
}

class PhysicalPrinter implements Printer {
  print(): void {}
}
```

The review must ask:

```text
Do these implementations keep the same behavioural promise?
```

Check whether implementations differ in meaningful ways:

- one silently does extra work
- one throws errors the contract did not describe
- one mutates state unexpectedly
- one requires prior setup not required by the original
- one depends on external services when the original did not
- one changes return meaning
- one weakens guarantees
- one refuses valid inputs accepted by the base
- one accepts invalid inputs and hides the issue

The compiler may be satisfied while the design is still broken.

---

## Override Warning

Overrides are not automatically wrong.

Overrides are a warning point.

When reviewing an override, ask:

```text
Is this extending expected behaviour, or replacing the original promise with a different promise?
```

A substitute becomes suspicious when it overrides base behaviour by:

- removing expected behaviour
- weakening guarantees
- changing side effects
- returning incompatible results
- throwing unexpected exceptions
- requiring new preconditions
- ignoring required state
- changing the domain meaning of the operation

Example:

```text
Bird.fly()
Penguin.fly() -> not implemented
```

This does not prove that Penguin is wrong.

It proves the base abstraction probably lied.

If not all birds fly, flight does not belong on the base Bird abstraction.

Move the behaviour to a more accurate abstraction, such as a Flyable/FlyingBird capability, rather than forcing non-flying birds to fake compatibility.

---

## Side Effects

Side effects are part of the behavioural contract.

Given:

```ts
interface UserStore {
  save(user: User): void;
}
```

An implementation that saves a user fulfils the obvious contract.

An implementation that saves a user and publishes an event changes the behavioural promise.

An implementation that saves a user, publishes an event, writes audit logs, and updates cache changes the behavioural promise even more.

Do not treat hidden side effects as harmless implementation detail.

Ask:

```text
Would a consumer reasonably expect this side effect from the abstraction?
```

If no, the substitute is not trustworthy.

Unexpected side effects may indicate LSP failure and may also expose SRP, OCP, ISP, or DIP problems.

---

## Error and Exception Behaviour

Error behaviour is part of substitutability.

Given:

```ts
interface PaymentProvider {
  charge(): Result;
}
```

If the established contract is:

```text
returns Result.Success
returns Result.Failure
```

then an implementation that additionally throws `ValidationException` is not safely substitutable unless the contract explicitly allows that exception behaviour.

An implementation that also throws network, rate-limit, or provider-specific exceptions drifts even further from the original contract.

Ask:

```text
Could existing consumers safely handle this substitute without new error handling?
```

If no, the substitution is not safe.

Do not add unexpected exceptions to a substitute and pretend the interface is still the same.

---

## Null and Undefined

Returning `null` is not automatically an LSP violation.

Returning `undefined` is not automatically an LSP violation.

The question is:

```text
Was this value part of the original contract?
```

If the base abstraction allows `null`, a substitute may return `null` appropriately.

If the base abstraction guarantees a value, a substitute that returns `null` breaks that guarantee.

In TypeScript and JavaScript, be explicit about the difference between:

```text
no value
unknown value
not loaded
not applicable
not implemented
not found
```

Do not use `null` or `undefined` to smuggle incompatible behaviour through a compatible-looking type.

---

## Type Checks and Special Cases

The presence of a type check is not automatically an LSP violation.

In weakly typed or structurally typed environments, runtime validation may be necessary.

However, type checks become suspicious when consuming code asks for a base abstraction and then immediately checks for specific derived implementations.

Example smell:

```ts
if (printer instanceof CloudPrinter) {
  // special behaviour
}
```

Ask:

```text
Why does the consumer need to know the concrete implementation?
```

If the consumer needs different logic for different substitutes, the substitutes may not share a trustworthy abstraction.

This may also indicate missing interfaces, poor decomposition, or responsibilities in the wrong place.

---

## Performance and Time

Performance is not automatically an LSP concern.

For asynchronous operations, different implementations may take different amounts of time.

Example:

```text
PdfPrinter.print() -> 50ms
CloudPrinter.print() -> 45 seconds
```

This does not automatically break LSP if the abstraction is explicitly asynchronous and consumers are expected to await completion.

Performance becomes relevant only when timing is part of the behavioural contract.

Ask:

```text
Does the abstraction promise a timing, timeout, synchrony, ordering, or responsiveness guarantee?
```

If yes, substitutes must honour that guarantee.

If no, treat latency as a non-functional concern rather than a direct LSP failure.

---

## Interface Drift

Interfaces can lie.

A derived or extended interface may appear compatible while changing the practical meaning of the contract.

Be suspicious when a derived interface:

- changes property meaning
- widens accepted values unexpectedly
- narrows accepted values unexpectedly
- adds nullable values where none existed
- changes return semantics
- changes optionality
- changes side-effect expectations
- overrides members in spirit even if the language allows it structurally

Example smell:

```ts
interface A {
  name: string;
  count: number;
  active: boolean;
}

interface B extends A {
  name: string | null;
  count: string | number;
  active: number;
}
```

Even if a language or workaround allows something similar, the contract has drifted.

Do not claim substitutability when the shape only survives through widening, narrowing, or reinterpretation.

---

## Union Types

Union types can break substitutability when they change what consumers must handle.

If original code expects:

```ts
string | number | boolean
```

and a substitute introduces:

```ts
string | number | boolean | null
```

then every consumer may now need to handle `null`.

Ask:

```text
Has this substitute added a new case that old consumers were not required to handle?
```

If yes, the substitution may no longer be safe.

Union expansion is still a contract change.

---

## Generics

Generic constraints are part of the contract.

Be suspicious when a substitute changes a generic from broad to narrow, or narrow to broad, in a way that affects consumers.

Ask:

```text
Can existing generic consumers still use this substitute without knowing the new type constraint?
```

If no, the substitute is not safely interchangeable.

Changing variance, constraints, or expected type bounds can create subtle LSP failures.

---

## Composition

Composition often reduces inheritance-related LSP problems, but it does not remove substitutability concerns.

A composed part still makes promises.

Example:

```text
Button
```

If a button can only work inside a specific card component, it may not be a general button.

Ask:

```text
Does this component claim to be reusable while secretly depending on one specific composition context?
```

If yes, the abstraction may be lying.

---

## Domain Meaning

Shared ancestry is not the same as shared purpose.

Do not force inheritance because two domain concepts appear related.

Ask:

```text
Do these concepts share behaviour, lifecycle, rules, state transitions, and consumer expectations?
```

If not, do not claim substitutability.

A derived type that overrides nearly everything from its base is usually evidence that the relationship is false.

If the substitute keeps the name but replaces the meaning, the design is lying.

---

## Practical Review Process

When reviewing a potential substitute:

1. Identify the original abstraction.
2. Identify the substitute.
3. List the promises made by the original abstraction.
4. Compare method signatures.
5. Compare accepted inputs.
6. Compare return values.
7. Compare error behaviour.
8. Compare side effects.
9. Compare state transitions.
10. Compare consumer assumptions.
11. Run, imagine, or request the shared base test suite.
12. Apply the Hot Swap Test.
13. Flag any unexpected behavioural difference.

Do not stop at type compatibility.

---

## AI-Generated Code Review

When reviewing AI-generated code, assume that substitutions may only be superficially correct.

Check for:

- interfaces implemented by classes with different behaviour
- derived classes overriding expected behaviour
- unexpected exceptions
- unexpected side effects
- widened union types
- narrowed generic constraints
- special-case checks in consumers
- subclasses that ignore or replace most base behaviour
- test doubles that do not behave like the real implementation

If generated code introduces a substitute, require evidence that it can safely replace the original.

---

## Relationship With Other SOLID Principles

LSP failures often expose other problems.

### SRP

If a substitute adds unrelated behaviour, it may also violate Single Responsibility.

### OCP

If a substitute exists only because the base abstraction was prematurely closed or incomplete, review Open Closed design.

### ISP

If a substitute has to fake unsupported methods, the interface may be too broad.

### DIP

If consumers depend on concrete substitute details, dependency direction may be wrong.

Do not misdiagnose every smell as LSP.

Use LSP to ask whether trust between abstraction and consumer still holds.

---

## Success Criteria

LSP is being applied correctly when:

- consumers can use substitutes without concrete knowledge
- substitutes pass the same behavioural tests as the base abstraction
- method signatures and behavioural promises both match
- side effects are compatible with the original contract
- error behaviour is compatible with the original contract
- derived types do not remove or invalidate base behaviour
- interfaces do not drift through hidden widening or narrowing
- consumers do not need special-case logic for specific implementations
- domain concepts are not forced into false inheritance relationships

The goal is simple:

```text
Swap implementations without surprising the caller.
```

---

## Core Mental Model

A likely LSP violation exists when:

```text
The consumer has to care which substitute it received.
```

If the consumer has to know, branch, defend, re-test, re-handle, or reinterpret behaviour for a specific substitute, the abstraction may not be trustworthy.

Trust is the centre of LSP.
