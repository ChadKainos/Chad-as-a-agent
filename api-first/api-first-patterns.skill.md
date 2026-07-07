# API-First Patterns Library

## Purpose

This library contains reusable API design patterns, anti-patterns, migration strategies, and review heuristics.

It is shared by:

- api-first-core.skill.md
- api-first-design.skill.md
- api-first-review.skill.md
- api-first-refactor.skill.md

The goal is consistency.

When multiple skills evaluate the same API they should reach broadly similar conclusions because they share the same underlying pattern language.

---

# Pattern: Small Truthful Surface

## Problem

Teams expose too much API too early.

## Principle

Start with the smallest surface that truthfully represents the consumer problem.

## Benefits

- easier adoption
- easier learning
- easier evolution
- fewer breaking changes

## Smells

- speculative features
- unused options
- future-proofing everywhere

---

# Pattern: Consumer-Owned Meaning

## Principle

The consumer owns business meaning.

The API owns structure and mechanics.

## Good

```ts
status: ApplicationStatus
```

## Weak

```ts
isComplete: boolean
```

when completion contains hidden business rules.

---

# Pattern: Default Presentation Never Meaning

## Principle

Defaults may reduce repetition.

Defaults must not silently make business decisions.

## Safe

- layout
- density
- styling

## Dangerous

- answers
- workflow state
- business outcomes

---

# Pattern: Composition Over Configuration

## Problem

Configuration grows until it absorbs every edge case.

## Solution

Provide composition points.

## Smells

- giant config objects
- endless flags
- nested options

---

# Pattern: Boolean Explosion

## Problem

Each new boolean doubles possible states.

## Review Questions

- are combinations valid?
- are combinations meaningful?
- should this be an enum?

## Favour

```ts
tone: 'success' | 'warning' | 'error'
```

---

# Pattern: Discriminated Union

## Purpose

Make valid states explicit.

## Example

```ts
type LinkAction = {
 kind: 'link';
 href: string;
};

type ButtonAction = {
 kind: 'button';
 onClick: () => void;
};
```

## Benefit

Invalid states become impossible.

---

# Pattern: Wrapper Feedback

## Principle

Repeated wrappers are evidence.

Do not treat wrappers as noise.

They indicate:

- missing abstraction
- poor defaults
- ownership problems
- usability issues

---

# Pattern: Configuration Absorbs Chaos

## Principle

When a config object constantly grows the abstraction is usually under strain.

## Signals

- one-off fields
- feature flags
- conditional rendering options
- organisation-specific behaviour

---

# Pattern: Design System Atom And Molecule

## Atom

Small reusable primitive.

Examples:

- button
- label
- input
- link

## Molecule

Composition of atoms.

Examples:

- date input
- summary list row
- search box

## Rule

Prefer stable atoms and reusable molecules.

---

# Pattern: Date Input

## Principle

Expose what the user sees.

If the user sees day, month and year separately, there is a strong argument for the API to preserve that distinction.

## Smell

Collapsing partial input into a Date too early.

---

# Pattern: Form Component

## Principle

Validation, business meaning and workflow decisions usually belong to consumers.

The component should focus on presentation and interaction.

---

# Pattern: Validation Ownership

## Principle

Validation should exist at the correct architectural level.

Avoid:

- business validation hidden in presentational components
- UI concerns leaking into validation libraries

---

# Pattern: Additive Evolution

## Principle

Prefer extending APIs to replacing APIs.

## Examples

- new enum values
- optional fields
- new methods
- new extension points

---

# Pattern: Deprecation Ramp

## Sequence

1. Introduce replacement
2. Document replacement
3. Mark legacy path deprecated
4. Migrate consumers
5. Remove legacy path

---

# Pattern: Compatibility Layer

## Purpose

Reduce migration pain.

Common forms:

- adapters
- wrappers
- aliases
- translation layers

---

# Pattern: Stable Naming

## Principle

Names become dependencies.

Rename only when the value outweighs migration cost.

## Review Question

Will this name still make sense in two years?

---

# Pattern: Contract First

## Principle

Design the relationship before implementation.

Questions:

- who consumes this?
- what are they expressing?
- what belongs to them?

---

# Pattern: Consumer Fluency

## Principle

Consumers should not need to negotiate with an API.

Warning signs:

- source code as documentation
- excessive setup
- confusing names
- unusual defaults

---

# Pattern: Resource Over Action

## REST Guidance

Prefer resource language.

Challenge endpoints like:

```http
POST /doThing
```

when a resource model would be clearer.

---

# Pattern: Event As Fact

## Principle

Events describe things that happened.

Events should not be disguised commands.

---

# Pattern: Platform Consumer Bias

## Principle

Optimise for adopters.

Platform teams often optimise for maintainers.

The consumer experience matters more.

---

# Pattern: Migration As Design

## Principle

A replacement API without a migration path is unfinished design.

Every refactor should explain:

- what changes
- why it changes
- how consumers move

---

# Pattern: Technical Debt Capture

## Principle

When compromises remain:

- name them
- explain them
- ticket them

Never hide them.

---

# Chad Heuristic Index

## The API Is The Relationship
Consumers live with contracts.

## The Wrapper Is Feedback
Repeated wrappers are evidence.

## Default Presentation Never Meaning
Ownership matters.

## Configuration Absorbs Chaos
Growing config surfaces are warnings.

## If It Needs A Paragraph The Name Is Wrong
Naming is design.

## Implementation Leakage In A Trench Coat
Public APIs should not expose internal decisions.

## Small Truthful Surface
Start small. Grow deliberately.

## Do Not Make Me Think
Fluent APIs disappear into the work.

## Migration Is Part Of Design
A better future requires a safe route to reach it.

---

# Selection Guidance

When designing:
- Contract First
- Consumer-Owned Meaning
- Small Truthful Surface
- Composition Over Configuration

When reviewing:
- Boolean Explosion
- Wrapper Feedback
- Consumer Fluency
- Stable Naming

When refactoring:
- Additive Evolution
- Compatibility Layer
- Deprecation Ramp
- Migration As Design
- Technical Debt Capture
