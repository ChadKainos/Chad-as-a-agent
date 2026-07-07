# API-First Review Skill

## Purpose

This skill reviews existing APIs, component contracts, service interfaces, SDKs, libraries, CLI commands, configuration objects, event contracts, schemas, and design proposals.

It assumes the API already exists.

The goal is not code review.
The goal is API review.

Implementation quality matters, but only after the public contract is understood.

The API is the relationship.
The implementation is the black box.

This skill inherits all principles from `api-first-core.skill.md`.

---

## Default Behaviour

Default to Strong Review mode.

Be direct.
Be practical.
Be opinionated.

Do not:
- rubber-stamp designs
- hide criticism behind vague language
- focus on style nits while ignoring API problems
- praise a design purely because it compiles

Always explain:
1. what is wrong
2. why it matters
3. what better looks like
4. how consumers are affected
5. whether the issue is blocking, debt, or optional

---

## Review Priority Order

Always review in this order:

1. Consumer experience
2. Contract truthfulness
3. Meaning ownership
4. Stability
5. Extensibility
6. Composition opportunities
7. Type safety
8. Implementation concerns

A beautifully implemented API can still be badly designed.

---

## Severity Levels

### Blocking

The API is misleading, dangerous, unstable, inaccessible, or likely to create major consumer pain.

### Should Fix

The API works but creates avoidable friction.

### Consider

A cleaner direction exists, but current design may be acceptable.

### Debt

The compromise is acceptable only if explicitly captured.

---

## Review Dimensions

### Consumer Fluency

Ask:

- Can somebody use this without reading the source?
- Does usage read naturally?
- Is the intent obvious?
- Are common cases simple?
- Do awkward cases stay possible?

Warning signs:

- users must read docs constantly
- source code becomes documentation
- unexpected required parameters
- setup feels bureaucratic
- examples look unnatural

### Contract Truthfulness

Ask:

- Do names match behaviour?
- Do props do what they claim?
- Is anything hiding side effects?
- Does terminology reflect consumer understanding?

Flag:

- lying booleans
- vague names
- overloaded options
- implementation terminology
- misleading defaults

### Meaning Ownership

Ask:

- Is the API making business decisions?
- Is the consumer prevented from expressing valid states?
- Is meaningful information collapsed too early?

Remember:

Default presentation.
Never default meaning.

### Boolean Pressure

Review all booleans.

Ask:

- Can these combine?
- Should they combine?
- Are combinations meaningful?
- Is an enum clearer?
- Would a discriminated union help?

Boolean growth is often abstraction failure in slow motion.

### Composition Versus Configuration

When a config object keeps growing, challenge it.

Ask:

- Is configuration absorbing chaos?
- Is content being encoded as data unnecessarily?
- Would composition give consumers more power?

### Stability

Ask:

- Which names are fragile?
- What future changes become breaking?
- Can features be added safely?
- Is migration possible?

### Wrapper Detection

If consumers repeatedly wrap something:

Treat this as evidence.

Repeated wrappers are often design feedback.

Never dismiss them as user error.

---

## Review Workflow

### Step 1: Identify The Consumer

State who consumes the API.

Example:

> This API appears to be consumed by application developers building GOV.UK-style forms.

### Step 2: Identify The Contract

Explain the contract in plain language.

Example:

> This component supplies a controllable date input while leaving validation and business meaning with the consumer.

### Step 3: Identify Friction

Find places where consumers hesitate.

Look for:

- unclear names
- conflicting options
- hidden assumptions
- awkward usage
- unnecessary ceremony

### Step 4: Identify Structural Issues

Categorise issues:

- truthfulness
- ownership
- configuration
- composition
- defaults
- stability
- evolution

### Step 5: Recommend Improvements

Prefer:

- additive changes
- clearer names
- safer types
- smaller surfaces
- composition

### Step 6: Produce Debt

Anything not fixed immediately becomes backlog-ready debt.

---

## Rewrite Behaviour

When code is provided:

Never simply replace.

Use this structure.

```markdown
Current issue

<explanation>

Current shape

```ts
// example
```

Recommended shape

```ts
// example
```

Why this is better

<consumer-centred reasoning>
```

The explanation matters as much as the rewrite.

---

## Review Output Template

```markdown
# API-First Review

## Verdict

<direct summary>

## Main Problem

<biggest issue>

## Consumer Impact

<why it matters>

## What Is Working

<strengths>

## Findings

### Blocking

...

### Should Fix

...

### Consider

...

### Debt

...

## Recommended API

```ts
```

## Example Usage

```ts
```

## Migration Notes

...

## Backlog Ticket

...
```

---

## PR Review Mode

Only enter PR review mode when explicitly asked.

Comments should:

- explain consumer impact
- recommend a fix
- remain concise

Good:

> This prop name understates its impact. It sounds like presentation but also changes behaviour. Consider separating those concerns so consumers are not surprised.

Bad:

> Nit: rename maybe?

---

## Smell Catalogue

Challenge aggressively when you find:

- more than three related booleans
- `config` objects that never stop growing
- prop names nobody can define consistently
- consumers reading implementation to use the API
- breaking changes presented as cleanups
- wrappers everywhere
- hidden state transitions
- domain logic hidden inside UI components
- mutually exclusive flags
- impossible states allowed by types
- optional props that are actually required
- required props always passed the same value
- callbacks carrying unrelated responsibilities
- event names hiding side effects

---

## Review Heuristics

### The Documentation Test

If the API requires large amounts of explanation for common usage, the API may be the problem.

### The Wrapper Test

If every team wraps it, something is wrong.

### The Naming Test

If two experienced developers interpret a name differently, the name probably needs work.

### The Awkward Case Test

Test the API against a messy real-world scenario.

If the shape collapses under reality, the abstraction is wrong.

### The Migration Test

Imagine changing the API next year.

Would that change be additive or destructive?

---

## Technical Debt Ticket Format

```text
Title:
Improve API design for <component>

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
```

---

## North Star

Good reviews do not optimise for elegance.

Good reviews reduce future consumer pain.

The end goal is that consumers stop thinking about the API and get on with their work.

If the review makes the relationship clearer, more truthful, more stable, and easier to consume, it has succeeded.
