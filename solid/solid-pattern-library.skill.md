---
name: "solid-pattern-library"
title: "SOLID Pattern Library"
description: "Guides comparison and application of known design patterns and recognition of anti-patterns without architecture cosplay."
version: "draft-2-agent-ready"
type: "agent-skill"
use_when: "Use when a known pattern may solve a problem space, or when comparing design options and avoiding pattern abuse."
activation: "Load when the user request matches use_when, or when another SOLID instruction file lists this file as a dependency."
depends_on:
  - "solid-core"
agent_consumption_notes:
  - "Read the metadata first, then follow the instructions as operating context."
  - "Treat imperative instructions as behaviour requirements for the agent."
  - "Do not describe this file as a third-party capability during normal user responses."
  - "Use the user's requested scope as the boundary for action unless the user explicitly expands it."
---

# SOLID Pattern Library

## Name

solid-pattern-library

## Description

Guides comparison and application of known design patterns and recognition of anti-patterns without architecture cosplay.

## When To Use

Use when a known pattern may solve a problem space, or when comparing design options and avoiding pattern abuse.

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

Use known design patterns and anti-patterns as practical engineering tools during SOLID review, code generation, and refactoring.

A SOLID pattern is useful when it can be reused in any domain and at any level to solve a common problem in an atomic way that composes with other patterns and non-patterned code.

Patterns are neutral.

Use makes a pattern helpful or harmful.

Do not recommend patterns as architecture cosplay. Recommend patterns only when they reduce complexity, coupling, fragility, risk, or future maintenance cost.

---

## Core Philosophy

Patterns do not fix code by existing.

Patterns are small engineering techniques with specific use cases and utility.

Treat patterns like fixings:

```text
nails
screws
glue
bolts
rivets
welds
```

All are ways of joining things, but each has a specific use case, strength, weakness, and failure mode.

Design patterns are the same.

Do not reach for a pattern because it is familiar.

Reach for a pattern because the problem space matches the pattern and the consequences are acceptable.

---

## Prime Directive

Use known existing solutions for common problems to stop the agent inventing novel solutions with novel risks.

Avoid burning tokens creating unnecessary new designs when a known, safe, reliable, well-understood solution already fits.

If no known pattern fits cleanly, do not force one.

Solve the problem plainly while staying as close as possible to SOLID, the tech stack's standards, and the codebase's norms.

---

## Pattern Use Rule

Recommend a pattern only when:

1. the problem space matches the pattern
2. the pattern reduces complexity, coupling, fragility, or risk
3. the trade-offs are understood
4. the negative consequences can be mitigated or prevented
5. the pattern fits the user's scope
6. the pattern fits the codebase and tech stack
7. the result is easier to maintain than the current design

If these are not true, do not recommend the pattern.

---

## Pattern Failure Rule

A pattern has failed when it increases:

- complexity
- coupling
- fragility
- risk
- maintenance effort
- bug surface area
- test difficulty
- scope creep

Any pattern that fails to solve the problem cleanly with minimal impact on the codebase is not a good fit and should not be used.

---

## Good Pattern vs Bad Pattern

A good pattern reduces:

```text
complexity
coupling
fragility
```

A bad pattern increases:

```text
complexity
coupling
fragility
```

The same named pattern can be good or bad depending on how, why, and where it is used.

---

## Pattern Recommendation Behaviour

Always compare options when recommending a pattern.

Do not simply say:

```text
Use Strategy.
```

Prefer:

```markdown
## Pattern Options

### Option 1: Strategy
- Benefits: ...
- Costs: ...
- Risks: ...

### Option 2: Pipeline
- Benefits: ...
- Costs: ...
- Risks: ...

### Option 3: Rules Engine
- Benefits: ...
- Costs: ...
- Risks: ...

## Recommendation
Use Strategy because...
```

If the decision is not clear, ask the user for more context rather than pretending certainty.

---

## Pattern Selection Starting Point

Start from the problem space.

Ask:

```text
What problem are we solving?
```

```text
What change are we protecting against?
```

```text
What complexity are we reducing?
```

```text
What coupling are we removing?
```

```text
What future change should become easier?
```

```text
What trade-offs does this pattern introduce?
```

Do not start from a favourite pattern.

When all someone has is a hammer, everything starts looking like a nail.

Avoid golden-hammer thinking.

---

## Pattern Categories

Recognise these problem categories equally. Do not assume one category is more important than another without context.

### Object Creation

Problems involving how objects, services, clients, or components are created.

Useful patterns may include:

- Factory Method
- Abstract Factory
- Builder
- Dependency Injection
- Composition Root
- Provider

Use only when creation complexity is real.

Do not create factory layers just to move `new` somewhere else.

---

### Behaviour Variation

Problems where behaviour changes while the caller should remain stable.

Useful patterns may include:

- Strategy
- State
- Template Method
- Command
- Visitor
- Rules Engine
- Policy Object

Prefer these when the variation is meaningful, expected, and needs to be isolated.

---

### External System Adaptation

Problems involving third-party APIs, infrastructure, SDKs, legacy systems, or incompatible interfaces.

Useful patterns may include:

- Adapter
- Facade
- Anti-Corruption Layer
- Gateway
- Proxy
- Wrapper

Use when the external system should not leak into domain or application logic.

---

### Responsibility Separation

Problems where one unit does too much or mixes concerns.

Useful moves may include:

- Extract Function
- Extract Class
- Coordinator
- Facade
- Service Object
- Mapper
- Presenter
- Domain Service

Use to make responsibilities truthful and easier to change independently.

---

### Workflow Orchestration

Problems involving multi-step processes, ordered operations, or controlled sequencing.

Useful patterns may include:

- Pipeline
- Chain of Responsibility
- Command
- Saga
- Process Manager
- Coordinator
- Workflow Object

Use when steps are meaningful and the sequence is part of the design.

Do not create a pipeline when a simple function is sufficient.

---

### Validation

Problems involving validation rules, input checks, policies, or conditional acceptance.

Useful patterns may include:

- Specification
- Validator
- Rules Engine
- Policy Object
- Chain of Responsibility
- Strategy

Use when validation rules vary, compose, or need to be tested independently.

---

### Mapping

Problems involving translation between domain, DTO, persistence, API, UI, or external contract shapes.

Useful patterns may include:

- Mapper
- Adapter
- Translator
- Assembler
- Anti-Corruption Layer

Use when shape conversion protects boundaries.

Do not hide business logic inside mappers.

---

### Persistence

Problems involving storage, retrieval, querying, and persistence boundaries.

Useful patterns may include:

- Repository
- Unit of Work
- Query Object
- Specification
- Data Mapper

Use when persistence details should not leak into business logic.

Do not create repositories that simply mirror an ORM without adding boundary value.

---

### Cross-Cutting Concerns

Problems involving logging, auditing, metrics, retries, caching, configuration, tracing, authentication, or authorisation.

Useful patterns may include:

- Decorator
- Middleware
- Interceptor
- Proxy
- Aspect-like wrappers
- Pipeline

Use when behaviour should be applied consistently without contaminating core responsibility.

---

### Testing Seams

Problems involving testability, isolation, substitution, or hard-to-control dependencies.

Useful patterns may include:

- Dependency Injection
- Adapter
- Port and Adapter
- Interface Extraction
- Test Double
- Fake
- Stub
- Mock
- Contract Test

Use when tests are painful because code depends on the wrong thing.

Do not introduce abstractions solely for mocks unless they also improve design.

---

## Common Useful Patterns

This list is not exhaustive. Use judgement.

### Adapter

Use when an existing implementation has the wrong shape but can be wrapped to match the expected contract.

Good for:

- external APIs
- legacy systems
- incompatible libraries
- printers/loggers/payment providers

Failure mode:

- adapter becomes a dumping ground
- adapter hides too much behaviour
- adapter introduces unexpected side effects

---

### Strategy

Use when a caller needs interchangeable behaviour behind a stable contract.

Good for:

- validation strategies
- pricing strategies
- sorting strategies
- export formats
- rules that vary by context

Failure mode:

- too many tiny strategies with no real variation
- strategy used instead of fixing poor decomposition

---

### Factory

Use when object creation is complex, conditional, or belongs at a creation boundary.

Good for:

- choosing implementations
- hiding construction complexity
- integrating with dependency injection

Failure mode:

- factory becomes a new keyword warehouse
- factory exists only to avoid learning dependency injection
- factory-of-factory layers appear

---

### Facade

Use when a simpler interface is needed over a more complex subsystem.

Good for:

- external integrations
- legacy areas
- complex subsystem boundaries

Failure mode:

- facade becomes a god service
- facade hides important domain behaviour
- facade grows into a dumping ground

---

### Repository

Use when persistence needs a boundary and consumers should not know storage details.

Good for:

- domain persistence
- query boundaries
- hiding database/ORM details

Failure mode:

- repository mirrors ORM with no value
- repositories become business services
- one repository tries to own several domains

---

### Mapper

Use when translating between data shapes across boundaries.

Good for:

- DTO to domain
- domain to API response
- persistence model to domain model

Failure mode:

- mapper becomes business logic
- mapper hides side effects
- mapper has multiple reasons to change

---

### Pipeline

Use when work is a series of meaningful, composable steps.

Good for:

- validation flows
- transformation flows
- middleware-like processing
- ETL-style workflows

Failure mode:

- pipeline exists for one simple operation
- steps become too tiny to understand
- data is repeatedly massaged just to fit the next step

---

### Decorator

Use when behaviour should be added around an existing contract without changing the original.

Good for:

- logging
- caching
- retry
- metrics
- tracing

Failure mode:

- hidden side effects surprise consumers
- decorator order becomes fragile
- too many layers obscure behaviour

---

### Coordinator

Use when a unit should orchestrate collaborators without owning their implementation details.

Good for:

- workflow control
- coordinating services
- sequencing operations

Failure mode:

- coordinator creates every dependency
- coordinator micromanages implementation details
- coordinator becomes a god object

---

### Composition

Use when behaviour should be assembled from smaller capabilities rather than inherited.

Good for:

- React components
- TypeScript services
- capability-based modelling
- functional pipelines

Failure mode:

- composition tree becomes unreadable
- dependencies are passed through too many layers
- composition hides ownership and responsibility

---

### Capability Interface

Use when only some implementations support an extra behaviour.

Good for:

- avoiding fake base methods
- LSP repair
- ISP repair

Example:

```text
Bird
Flyable
Swimmable
```

Failure mode:

- too many micro-capabilities
- consumers must manually assemble confusing combinations

---

## Anti-Pattern Categories

Actively recognise anti-patterns. Avoid recommending them.

Anti-patterns may look useful at first but create more harm than value.

Common categories include:

- design anti-patterns
- object-oriented anti-patterns
- programming anti-patterns
- methodological anti-patterns
- architecture anti-patterns
- configuration/deployment anti-patterns
- team/process anti-patterns
- security/compliance anti-patterns

Do not rely only on names. Understand the failure mode.

---

## Common Anti-Patterns To Watch For

### Golden Hammer

Using the same familiar solution for every problem.

Failure:

- pattern selected by habit rather than fit
- every problem becomes a nail

---

### God Object / God Class

One object knows too much or does too much.

Failure:

- violates SRP
- difficult to test
- difficult to change
- hard to reason about

---

### Big Ball of Mud

System lacks clear structure or boundaries.

Failure:

- change spreads unpredictably
- architecture becomes accidental

---

### Spaghetti Code

Code has tangled control flow and unclear structure.

Failure:

- hard to trace
- hard to test
- hard to safely change

---

### Shotgun Surgery

One change requires many edits across unrelated areas.

Failure:

- poor modularity
- tight coupling
- weak change boundaries

---

### Copy-Paste Programming

Duplicating code instead of extracting useful shared behaviour.

Failure:

- bugs repeated across copies
- maintenance cost multiplies

---

### Cargo Cult Programming

Applying code or patterns without understanding why.

Failure:

- solution appears familiar but does not fit the problem

---

### Service Locator Abuse

Pulling dependencies from global registries rather than declaring needs clearly.

Failure:

- hidden dependencies
- poor testability
- weak DIP

---

### Singleton Abuse

Using global single-instance objects as hidden dependencies.

Failure:

- hidden coupling
- test pollution
- state leaks

---

### Factory Everywhere

Creating factories for everything, even simple construction.

Failure:

- complexity increases
- creation policy becomes harder to trace

---

### Abstract Everything

Creating abstractions without substitution value.

Failure:

- extra layers
- harder navigation
- no change-protection benefit

---

### Inheritance Hell / Yo-Yo Problem

Deep inheritance chains require bouncing up and down hierarchy to understand behaviour.

Failure:

- fragile behaviour
- hard substitution
- high cognitive load

---

### Interface Bloat

Interfaces collect too many unrelated members.

Failure:

- consumers know too much
- ISP violation

---

### Anemic Domain Model

Domain objects carry data with little or no behaviour while behaviour spreads elsewhere.

Failure:

- business logic becomes scattered
- domain meaning weakens

---

### Circular Dependency

Modules or classes depend on each other in cycles.

Failure:

- hard to build
- hard to test
- hard to change independently

---

### Hard Coding

Embedding volatile values or decisions directly in code.

Failure:

- future change requires code edits
- poor configurability

---

### Magic Numbers / Magic Strings

Using unexplained literal values.

Failure:

- unclear intent
- repeated hidden meaning

---

### Lava Flow / Dead Code

Old code remains because nobody knows whether it is safe to remove.

Failure:

- confusion
- accidental reuse
- maintenance drag

---

### Premature Optimisation

Optimising before understanding performance bottlenecks.

Failure:

- complexity increases without evidence

---

### Overengineering

Adding design complexity beyond current or credible future need.

Failure:

- harder tests
- harder maintenance
- unclear ownership

---

### Analysis Paralysis

Design discussion prevents useful progress.

Failure:

- no value delivered
- decisions delayed without learning

---

### Scope Creep

Solution expands beyond the requested change without control.

Failure:

- token burn increases
- user scope is lost
- delivery risk increases

---

### Not Invented Here

Rejecting existing suitable solutions because they were not created locally.

Failure:

- duplicate effort
- avoidable complexity

---

### Dependency Hell

Dependency versions or dependency relationships become difficult to manage.

Failure:

- fragile builds
- upgrade pain
- forced compromises

---

### Vendor Lock-In

Design depends too tightly on one external provider.

Failure:

- future movement becomes expensive
- external changes dictate internal architecture

---

## Abstraction Rule

Do not abstract until the abstraction provides value by simplifying use for consuming code.

A useful abstraction:

- hides irrelevant detail
- protects consumers from change
- improves testability
- improves substitutability
- reduces duplication of semantic concepts
- makes intent clearer

A useless abstraction:

- adds names but no clarity
- hides simple code behind ceremony
- exists only for mocks
- exists because a pattern was fashionable
- increases navigation cost

---

## Language and Stack Guidance

Most design patterns are universal.

Do not create separate pattern rules for every language by default.

Use language-specific examples only when helpful.

Some patterns are tech-stack or language idioms rather than universal design patterns. Treat those as stack-specific adaptations.

Examples:

- React higher-order components
- React hooks
- Express middleware
- TypeScript discriminated unions
- TypeScript structural interfaces
- Node.js dependency wrappers
- C# LINQ query objects

Use stack idioms when they solve the same problem more naturally than a generic pattern.

---

## TypeScript / Node / React Guidance

Keep patterns mostly language-agnostic, but adapt examples to the stack under review.

For TypeScript:

- prefer clear structural contracts
- avoid fake interfaces with no substitution value
- use discriminated unions where they express bounded variation clearly
- avoid widening types just to force compatibility

For Node and Express:

- avoid route handlers constructing infrastructure directly
- use adapters for external SDKs
- keep middleware focused
- avoid service dumping grounds

For React:

- treat props as interfaces
- use composition before inheritance-style thinking
- avoid passing whole domain objects when components need small display contracts
- avoid context as a global dumping ground

For tests:

- use patterns to create seams
- do not abstract solely to mock
- prefer contract tests where substitutability matters

---

## Pattern Comparison Template

Use this template when recommending a pattern.

```markdown
## Problem
<What design problem needs solving.>

## Options Considered

### Option 1: <Pattern>
Benefits:
- <benefit>

Costs/Risks:
- <risk>

### Option 2: <Pattern>
Benefits:
- <benefit>

Costs/Risks:
- <risk>

## Recommendation
<Recommended option and why.>

## Why This Fits
<How it reduces complexity, coupling, fragility, or risk.>

## When Not To Use It
<Conditions that would make this pattern a bad fit.>
```

---

## Relationship With SOLID Principles

Do not map patterns rigidly to single SOLID principles.

Patterns match problem spaces, not SOLID letters.

Any mapping from pattern to principle is contextual and partly personal judgement.

However, patterns can support SOLID outcomes:

- Adapter can support DIP, LSP, ISP, and OCP depending on use
- Strategy can support OCP and SRP depending on use
- Extract Class can support SRP but can also harm ISP if overdone
- Repository can support DIP but can become a dumping ground
- Decorator can support OCP but can create hidden side effects
- Capability Interface can support ISP and LSP

Always reason from the problem, not from a fixed pattern-to-principle table.

---

## When No Pattern Fits

If no known pattern fits cleanly:

1. Do not force a pattern name.
2. Describe the design shape plainly.
3. Solve the problem as simply as possible.
4. Stay close to SOLID.
5. Stay close to stack standards and codebase norms.
6. Ask the user for more input if needed.

A nameless simple design is better than a misapplied famous pattern.

---

## AI-Generated Code Behaviour

When generating or reviewing code:

- avoid inventing novel architecture when a known pattern fits
- avoid choosing the statistically common pattern if it is an anti-pattern in context
- compare options before recommending
- explain trade-offs when the choice matters
- keep scope controlled
- prefer boring reliable designs over clever novelty

Do not let token generation drift into novel designs with novel problems.

---

## Do Not

Do not recommend a pattern by name without checking fit.

Do not assume a common pattern is safe in the current context.

Do not use patterns to hide missing domain understanding.

Do not use patterns as a substitute for decomposition.

Do not create abstractions that do not simplify consuming code.

Do not map patterns mechanically to SOLID letters.

Do not ignore anti-patterns because they look like familiar patterns.

Do not recommend clever designs when simpler code is safer.

---

## Success Criteria

Pattern library guidance is being followed when:

- the problem space is understood before a pattern is suggested
- multiple pattern options are compared when relevant
- positives and negatives are weighed
- the selected pattern reduces complexity, coupling, fragility, or risk
- the pattern fits the user's scope
- the pattern fits the codebase and tech stack
- anti-patterns are recognised and avoided
- no pattern is forced where none fits
- the result remains easier to maintain and test

---

## Core Summary

```text
Patterns are neutral.
```

```text
Use makes a pattern good or bad.
```

```text
A useful pattern solves a common problem atomically and composes with other code.
```

```text
A good pattern reduces complexity, coupling, and fragility.
```

```text
A bad pattern increases complexity, coupling, and fragility.
```

```text
Patterns match problem spaces, not SOLID principles.
```

```text
If no pattern fits cleanly, do not force one.
```
