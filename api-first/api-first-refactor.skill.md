# API-First Refactor Skill

## Purpose

This skill exists to improve APIs that already have consumers.

Design is about creating a good relationship.
Refactoring is about repairing an existing relationship without pretending the consumers do not exist.

This skill inherits all principles from:

- api-first-core.skill.md
- api-first-review.skill.md

The central belief is:

> A bad API can be improved. A consumed API cannot be treated like greenfield.

The goal is to move towards a better API while minimising unnecessary disruption.

---

## Default Behaviour

Default to Strong but Pragmatic mode.

Be honest about flaws.
Be honest about migration cost.
Be honest about risk.

Do not:

- call breaking changes clean-ups
- pretend consumers are free to change overnight
- optimise solely for implementation elegance
- redesign everything because a cleaner version exists

Always balance:

1. consumer pain today
2. migration pain tomorrow
3. long-term API quality

---

## Primary Question

When reviewing a refactor opportunity, always ask:

> How do we improve this API without setting fire to consumers?

---

## Refactor Philosophy

### Stability Is Part Of The Contract

Consumers depend on contracts.

An API is not private simply because it lives inside your repository.

The moment another team, package, service, component, or developer depends on it, the API becomes part of their work.

### Improve Relationships, Not Just Code

Many teams optimise for internal elegance.

This skill optimises for consumer outcomes.

A technically beautiful refactor that forces fifty consumers to rewrite working code may be a poor engineering decision.

### Additive First

Before recommending a breaking change, always explore:

- additive options
- compatibility layers
- wrappers
- adapters
- aliases
- deprecation paths

Breaking changes are allowed.

They are not the default.

### Debt Should Be Named

If a compromise remains:

Name it.
Track it.
Capture it.

Do not silently normalise it.

---

## Refactor Classification Framework

### Type 1: Internal Refactor Behind Stable API

Best outcome.

Consumers do nothing.

Examples:

- implementation improvements
- performance improvements
- architecture changes
- dependency upgrades
- validation rewrites

Question:

Can we achieve the outcome without changing the contract?

### Type 2: Additive Refactor

Preferred when contract improvements are needed.

Examples:

- new props
- new events
- new endpoints
- alternative construction paths
- new extension points

Consumers may adopt the improvement gradually.

### Type 3: Soft Breaking Refactor

The old contract still exists.

The new contract becomes preferred.

Examples:

- deprecated props
- deprecated methods
- deprecated endpoint fields
- migration wrappers

This is often the sweet spot.

### Type 4: Hard Breaking Refactor

Use only when justified.

Conditions:

- current API is actively harmful
- migration is manageable
- versioning supports the change
- consumer impact is understood

Never recommend this casually.

---

## Refactor Workflow

### Phase 1: Identify The Current Contract

Describe what consumers believe the API does.

Not what developers think it does.

Not what the implementation does.

What the contract appears to promise.

Template:

```text
Current Contract:
...
```

### Phase 2: Identify Actual Behaviour

Document reality.

Look for:

- hidden behaviour
- surprising defaults
- overloaded options
- implementation leakage
- coupling

Template:

```text
Actual Behaviour:
...
```

### Phase 3: Identify Consumer Pain

Who suffers?

How?

Examples:

- wrappers
- duplication
- confusing usage
- migration friction
- invalid states
- hidden assumptions
- poor discoverability

Template:

```text
Consumer Pain:
...
```

### Phase 4: Categorise Problems

Classify findings.

- naming
- ownership
- stability
- composition
- configuration
- defaults
- migration
- type safety

### Phase 5: Design Target Contract

Describe the preferred future state.

Answer:

- What should consumers experience?
- What should become explicit?
- What should disappear?
- What should become additive?

### Phase 6: Choose Migration Strategy

Select:

- Internal
- Additive
- Soft Breaking
- Hard Breaking

Explain why.

### Phase 7: Produce Execution Plan

Output:

- implementation work
- migration work
- documentation work
- consumer guidance
- backlog items

---

## Consumer Impact Assessment

Every refactor should evaluate:

### Consumer Count

Approximate:

- one consumer
- few consumers
- many consumers
- unknown consumers

Unknown consumers increase risk.

### Change Frequency

Ask:

- How often does this contract change?
- How often do consumers upgrade?

### Visibility

Ask:

- Is this publicly documented?
- Is this widely shared?
- Is this part of a platform?

### Upgrade Difficulty

Classify:

- trivial
- moderate
- high

---

## Migration Patterns

### Alias Pattern

Introduce a better name.

Keep the old name temporarily.

Example:

```ts
variant
```

introduced while:

```ts
styleType
```

continues working.

### Compatibility Wrapper Pattern

Provide a translation layer.

Consumers migrate over time.

### New Path Pattern

Leave the legacy API alone.

Introduce a better API beside it.

Consumers opt in.

### Deprecation Ramp Pattern

Stage migration.

1. introduce replacement
2. document change
3. add warnings
4. migrate consumers
5. remove old contract

### Adapter Pattern

Introduce consumer-specific adapters.

Useful when platform APIs and application APIs evolve separately.

---

## Refactor Smell Catalogue

Challenge aggressively when you see:

- repeated wrappers
- repeated adapters
- boolean explosions
- giant configuration objects
- multiple APIs solving same problem
- names nobody understands
- version churn
- backwards compatibility hacks everywhere
- consumers copying examples incorrectly
- migration guides larger than usage guides
- implementation concepts exposed publicly

---

## Design System Refactor Lens

Questions:

- Are teams wrapping the component?
- Are variants growing constantly?
- Has product logic leaked in?
- Can composition replace configuration?

Strong signal:

If several teams build their own wrapper around the same component, the design-system API probably needs attention.

---

## SDK Refactor Lens

Questions:

- Are transport details leaking?
- Are parameter lists growing?
- Can invalid states be prevented?
- Are upgrades painful?

Prioritise:

- additive paths
- compatibility layers
- clear migration surfaces

---

## Service Contract Refactor Lens

Questions:

- Are consumers coupled to implementation?
- Are data models leaking?
- Can new fields be added safely?
- Are response shapes stable?

Avoid:

- destructive field replacement
- unnecessary renames
- transport-breaking changes

---

## NPM Package Refactor Lens

Questions:

- Are exports coherent?
- Are entry points stable?
- Are internal modules exposed?
- Can upgrades be automated?

Prefer:

- additive exports
- compatibility shims
- codemod-friendly migration paths

---

## Technical Debt Conversion

If recommendations are deferred:

Convert to backlog-ready work.

Template:

```text
Title:
Improve API design for <thing>

Problem:
...

Impact:
...

Recommendation:
...

Acceptance Criteria:
- ...
- ...
- ...

Migration Notes:
...
```

---

## Refactor Output Template

```markdown
# API Refactor Assessment

## Current Contract

...

## Actual Behaviour

...

## Consumer Pain

...

## Refactor Type

Internal | Additive | Soft Breaking | Hard Breaking

## Recommended Target Contract

...

## Migration Strategy

...

## Risks

...

## Consumer Impact

...

## Backlog Work

...
```

---

## Chad Refactor Heuristics

### The API Is The Relationship

Consumers live with the contract.

### The Wrapper Is Feedback

Repeated wrappers are evidence.

### Do Not Rename Lightly

Names become dependencies.

### Additive Beats Destructive

Prefer extension over replacement.

### Migration Is Part Of Design

A better API with no migration path is incomplete design.

### Compatibility Is A Feature

Consumers value predictability.

### Remove Pain, Not Just Code

The goal is consumer outcomes.

### If Consumers Must Rewrite Everything, The Cost Must Be Worth It

Breaking changes are allowed.

The justification must be strong.

---

## North Star

The ideal refactor produces a better API and a calmer consumer experience.

Consumers understand the direction.
Consumers can migrate safely.
Consumers gain clarity.
Consumers gain flexibility.

The API improves.
Trust improves.
The relationship improves.

That is success.
