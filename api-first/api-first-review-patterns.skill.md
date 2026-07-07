# API-First Review Patterns Library

## Purpose

This library extends api-first-review.skill.md.

The review skill provides the review engine.
This library provides specialised review lenses.

Different APIs fail in different ways.

A React component should not be reviewed the same way as a REST contract.
A pipeline template should not be reviewed the same way as an event schema.
A design-system component should not be reviewed the same way as a CLI.

When a review target matches one of the patterns below, automatically apply the relevant pattern in addition to the core API-First principles.

---

# Pattern: React Component API Review

## Primary Concern
Consumer fluency.

## Key Questions
- Can I use this without reading source code?
- Are prop names truthful?
- Are meaningful choices owned by the consumer?
- Would consumers build wrappers?
- Is composition being avoided?

## Smells
- Boolean explosion
- Giant props objects
- Business logic in presentation APIs
- Render props used as escape hatches
- Children ignored in favour of configuration
- Implementation concepts exposed publicly

## Chad Heuristic
If a component needs a diagram to explain its props, the API is probably failing.

---

# Pattern: React Hook Review

## Primary Concern
Behavioural clarity.

## Key Questions
- Does the name describe the responsibility?
- Are side effects obvious?
- Is ownership of state clear?
- Are consumers forced to understand implementation?

## Smells
- Kitchen sink hooks
- Hidden network calls
- Hidden navigation
- Hidden mutations
- Excessive return values

## Chad Heuristic
Hooks are APIs. Review them like APIs.

---

# Pattern: REST API Review

## Primary Concern
Contract stability.

## Key Questions
- Does the endpoint model consumer intent?
- Can new fields be added safely?
- Are consumers coupled to storage design?
- Does naming reflect business language?

## Smells
- Action-heavy endpoints
- Database leakage
- Generic payload blobs
- Versioning panic
- Multiple response shapes for the same request

## Chad Heuristic
Consumers remember contracts. They do not care about your persistence layer.

---

# Pattern: GraphQL Review

## Primary Concern
Schema evolution.

## Key Questions
- Are types stable?
- Are fields named around consumer concepts?
- Can consumers retrieve only what they need?
- Are resolver details leaking into the schema?

## Smells
- Generic scalar abuse
- Nested monsters
- Transport leakage
- Resolver-driven naming

---

# Pattern: SDK Review

## Primary Concern
Ergonomics.

## Key Questions
- Is the happy path obvious?
- Do method names match user intent?
- Are parameter lists manageable?
- Can the type system prevent mistakes?

## Smells
- Parameter graveyards
- Transport leakage
- Generic method naming
- Giant configuration objects

---

# Pattern: CLI Review

## Primary Concern
Discoverability.

## Key Questions
- Can commands be guessed?
- Are flags obvious?
- Are subcommands consistent?
- Is help supporting the API or rescuing it?

## Smells
- Mystery flags
- Single command doing everything
- Inconsistent naming
- Hidden defaults affecting behaviour

---

# Pattern: Event Contract Review

## Primary Concern
Meaning preservation.

## Key Questions
- Is the event a fact?
- Can consumers understand it independently?
- Is meaning explicit?
- Are names stable?

## Smells
- Commands disguised as events
- Generic payload objects
- Producer-centric naming
- Reused event types with multiple meanings

## Chad Heuristic
Events should describe something that happened, not something you hope somebody else will do.

---

# Pattern: Configuration Schema Review

## Primary Concern
Comprehensibility.

## Key Questions
- Does every setting have a clear meaning?
- Are settings independent?
- Are combinations valid?
- Can owners explain settings without reading code?

## Smells
- Nested doom
- Flag forests
- Mutually exclusive settings
- Growth without structure

---

# Pattern: Design System Component Review

## Primary Concern
Long-term organisational reuse.

## Key Questions
- Is this solving one problem well?
- Is business logic leaking in?
- Are teams wrapping it?
- Are variants multiplying?

## Smells
- Product-specific props
- Team-specific variants
- Accessibility hidden internally
- Every consumer wanting an exception

## Chad Heuristic
A design-system component should not know which project is using it.

---

# Pattern: Azure DevOps Pipeline Template Review

## Primary Concern
Consumer predictability.

## Key Questions
- Is the template understandable without opening internals?
- Are parameters consumer-focused?
- Is behaviour explicit?
- Can teams adopt it safely?

## Smells
- Magic variables
- Hidden conditions
- Boolean-heavy template parameters
- Internal resource naming leakage
- Parameter combinations nobody understands

---

# Pattern: NPM Package Review

## Primary Concern
Ease of adoption.

## Key Questions
- Is usage obvious?
- Are exports coherent?
- Are package boundaries clear?
- Can consumers understand upgrades?

## Smells
- Barrel file chaos
- Multiple competing entry points
- Breaking changes disguised as refactors
- Internal modules exposed publicly

---

# Pattern: Validation Library Review

## Primary Concern
Truthful constraints.

## Key Questions
- Do validation rules match business meaning?
- Are error models usable?
- Is validation separated from presentation?

## Smells
- Validation hidden in UI components
- Generic error blobs
- Validation coupled to storage format

---

# Pattern: Microservice Boundary Review

## Primary Concern
Service relationships.

## Key Questions
- Is ownership clear?
- Are responsibilities separated?
- Are consumers coupling to implementation?

## Smells
- Shared database assumptions
- Leaking internal identifiers
- Service boundaries based on org charts

---

# Pattern: LLM Agent Skill Review

## Primary Concern
Behavioural contracts.

## Key Questions
- What responsibility does the agent own?
- Is scope clear?
- Are instructions conflicting?
- Can behaviour evolve safely?

## Smells
- Giant prompt blobs
- Multiple responsibilities in one skill
- Undefined escalation paths
- Hidden assumptions

## Chad Heuristic
If a skill needs 40 exceptions, the abstraction is probably wrong.

---

# Universal Chad Heuristics

Apply to every review regardless of category.

## The API Is The Relationship
Consumers live with APIs.

## The Wrapper Is Feedback
Repeated wrappers are evidence.

## Default Presentation, Never Meaning
Meaning belongs to consumers.

## Configuration Absorbs Chaos
Growing configuration surfaces are a warning sign.

## If It Needs A Paragraph, The Name Is Wrong
Naming is design.

## Small Truthful Surface
Start small. Grow deliberately.

## Do Not Make Me Think
Fluent APIs are easier to trust.

## Implementation Leakage In A Trench Coat
Any API exposing internals has probably hidden the real design problem.

---

# Pattern Selection Matrix

If reviewing:

- React component -> React Component Review
- React hook -> React Hook Review
- REST endpoint -> REST Review
- GraphQL schema -> GraphQL Review
- SDK -> SDK Review
- CLI -> CLI Review
- Event schema -> Event Review
- Config file -> Configuration Review
- Design-system component -> Design System Review
- Pipeline template -> Pipeline Review
- NPM package -> Package Review
- Validation framework -> Validation Review
- Service boundary -> Microservice Review
- Agent skill -> LLM Agent Review

Apply the specialised pattern first.
Then apply the universal heuristics.
