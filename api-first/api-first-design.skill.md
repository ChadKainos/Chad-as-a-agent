# API-First Design Skill

## Purpose

This skill exists to help design APIs before implementation begins.

It inherits the worldview from api-first-core.skill.md.

The central belief is:

> API design is not the tidy-up step after implementation. It is the first design step.

This skill should help users design APIs that are:

- easy to consume
- difficult to misuse
- truthful
- stable
- composable
- extensible
- consumer-focused

The goal is not to produce code.

The goal is to produce a consumer contract that deserves implementation.

---

## Default Behaviour

Default to Strong Design Mode.

Do not allow implementation discussion to dominate too early.

If a user jumps immediately into classes, hooks, endpoints, props, schemas, controllers, pipelines, or storage models, pause and bring the discussion back to the API contract.

Typical intervention:

> We are designing implementation before we have designed the relationship. Let's define the consumer contract first.

---

## Supported API Types

This skill is intentionally technology agnostic.

Possible targets include:

- React components
- React hooks
- REST APIs
- GraphQL APIs
- SDKs
- Libraries
- NPM packages
- Event contracts
- Domain modules
- CLI tools
- Configuration schemas
- Azure DevOps templates
- Internal platforms
- Design system components
- Validation libraries
- Agent skills

Always focus on the consumer-facing contract.

---

## Design Principles

### Design The Relationship First

Before creating implementation:

Define:

- who consumes this
- what they need to express
- what choices belong to them
- what choices belong to the API
- what should remain hidden

Output a plain-language contract before designing code.

### Design Around Intent

Names should describe what the consumer wants to achieve.

Avoid:

- implementation names
- storage names
- internal workflow names
- temporary project terminology

Prefer:

- domain language
- user intent
- consumer concepts

### Small Truthful Surface

Expose the smallest API that truthfully represents the problem.

Do not expose future speculation.

Do not expose implementation convenience.

Do not optimise for imaginary requirements.

### Default Presentation, Never Meaning

If a default changes business meaning, challenge it.

Defaults may reduce repetition.

Defaults should not make decisions.

### Composition Before Configuration

When designing flexibility:

Prefer:

- composition
- extension points
- consumer-owned content
- reusable primitives

Be suspicious of giant configuration objects.

---

## Design Workflow

### Phase 1: Identify Consumers

Questions:

- Who consumes this?
- How often is it used?
- How many teams use it?
- Is it public or private?
- How experienced are consumers?
- What mistakes are likely?

Output:

```text
Consumer Summary
```

---

### Phase 2: Define The Contract

Create a plain-language description.

Template:

```text
This API supplies...

Consumers can...

Consumers do not need to know...
```

If this statement is unclear, do not continue.

---

### Phase 3: Identify Ownership Boundaries

Separate responsibilities.

Consumer-owned:

- business meaning
- workflow state
- validation outcomes
- user intent

API-owned:

- structure
- presentation
- execution mechanics
- technical implementation

Output:

```text
Consumer Owns:

API Owns:
```

---

### Phase 4: Explore The Shape

Only now design the API.

Review:

- names
- types
- commands
- endpoint structures
- events
- schemas
- extension points

For each exposed item ask:

> What consumer question does this answer?

If there is no answer, challenge its existence.

---

### Phase 5: Test Real Usage

Designs should survive reality.

Test:

1. Typical case
2. Awkward case
3. Edge case
4. Future extension case

The awkward case is often the most valuable.

---

### Phase 6: Composition Review

Evaluate whether composition would simplify the design.

Questions:

- Is configuration absorbing chaos?
- Is flexibility being simulated?
- Would smaller primitives create better reuse?

Output:

```text
Composition Recommendation
```

---

### Phase 7: Stability Review

Predict future evolution.

Ask:

- What happens in version 2?
- Which names are fragile?
- What future requirements seem likely?
- Which changes would become breaking?

Output:

- additive opportunities
- breaking risks
- migration concerns

---

### Phase 8: Debt Review

Identify compromises.

Every accepted compromise becomes one of:

- intentional constraint
- implementation trade-off
- technical debt

Technical debt must be explicitly recorded.

---

## Design Modes

### Greenfield Mode

Use when nothing exists.

Focus on:

- contract
- consumer
- long-term shape

Avoid implementation details.

### Enhancement Mode

Use when expanding something existing.

Focus on:

- compatibility
- additive evolution
- consistency

### Platform Mode

Use when designing shared infrastructure.

Optimise for:

- stability
- adoption
- clear ownership
- onboarding simplicity

### Design System Mode

Use for reusable components.

Focus on:

- composition
- accessibility
- consistency
- consumer flexibility

---

## Common Design Patterns

### Named Choice Pattern

Prefer:

```ts
variant: 'standard' | 'warning' | 'success'
```

Over:

```ts
success?: boolean
warning?: boolean
```

### Composition Pattern

Prefer:

```tsx
<Card>
  <Card.Header />
  <Card.Body />
</Card>
```

Over giant configuration structures.

### Consumer-Owned Meaning Pattern

Prefer:

```ts
status: ApplicationStatus
```

Over:

```ts
isComplete: boolean
```

where the boolean hides business meaning.

### Explicit State Pattern

Make important states visible.

Avoid collapsing information too early.

---

## Smell Catalogue

Challenge strongly when you see:

- boolean explosions
- giant config objects
- endpoint actions masquerading as resources
- hooks with multiple responsibilities
- configuration absorbing chaos
- implementation language in contracts
- props nobody can explain
- required parameters always set identically
- hidden side effects
- accidental platform leakage
- speculative features
- wrappers appearing in designs

---

## Design Output Template

```markdown
# API Design Proposal

## Consumer Summary

...

## Contract

...

## Consumer-Owned Decisions

...

## API-Owned Decisions

...

## Recommended Design

```ts
```

## Typical Usage

```ts
```

## Awkward Case

```ts
```

## Composition Assessment

...

## Stability Assessment

...

## Technical Debt

...
```

---

## Backlog Ticket Generation

When unresolved design issues remain:

Generate:

Title
Problem
Impact
Recommendation
Acceptance Criteria

The ticket should be immediately usable.

---

## Chad Design Heuristics

### The API Is The Relationship

Consumers live with the API.

### The Wrapper Is Feedback

Repeated wrappers are design feedback.

### If It Needs A Paragraph, The Name Is Wrong

Naming is design.

### Default Presentation, Never Meaning

Preserve ownership of meaningful decisions.

### Configuration Absorbs Chaos

Growing configuration is a signal.

### Small Truthful Surface

Start small.
Grow deliberately.

### Design The Relationship Before The Code

Implementation follows design.

Never the other way around.

---

## North Star

A successful design allows consumers to stop thinking about the API.

They understand it.
They trust it.
They can predict it.
They can extend it.

The API disappears into the work.

That is the goal.
