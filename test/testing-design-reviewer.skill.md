# Skill: Testing Design Reviewer

## Purpose

Review automated tests for reliability, maintainability, isolation, observability and design quality.

Focus on engineering principles rather than framework-specific syntax.

The goal is to identify risks that reduce confidence in a test suite, introduce flakiness, increase maintenance cost or create false confidence in system behaviour.

---

## Core Philosophy

Good tests create confidence.

Bad tests create doubt.

Flaky tests create folklore.

A test exists to answer a question about a system.

The role of a test is to observe reality, not influence it.

A test should be a witness, not a participant.

The purpose of a test is not to prove that code can work.

The purpose of a test is to prove that behaviour does work.

---

## Non-Goals

Do not review:

- naming conventions
- formatting
- code style
- assertion library choice
- framework preferences
- file structure preferences

unless they directly impact reliability, maintainability or test design.

---

# Review Principles

## Observability

- Assertions should not change the state of the system they are observing.
- Tests should observe reality, not manufacture it.
- The test should answer a question, not influence the answer.

## Reliability

- A test should be deterministic or it should not exist.
- Shared state is where reliable tests go to die.
- Waiting is not synchronisation. It is hope with a timeout.
- Poll for a condition, never for a duration.
- A queue is not ordered from the perspective of a test unless the system explicitly guarantees it.
- If developers do not trust a test, the test is already failing.

## Isolation

- Tests should pass when executed individually.
- Tests should pass when executed in parallel.
- Tests should pass regardless of execution order.
- One test should never determine the outcome of another.

## Maintainability

- A failing test should describe the problem, not create a puzzle.
- A test should fail for one reason, not many.
- The setup should be smaller than the assertion.
- Test data should clarify intent, not demonstrate creativity.
- A test should explain the system to its future maintainers.

## Design

- Tests should verify behaviour, not implementation details.
- The more a test knows about internals, the more fragile it becomes.
- Tests should prove something important, not something possible.
- The purpose of a mock is isolation, not fiction.

## Assertions

- Assertions should fail loudly when behaviour is wrong.
- Weak assertions create false confidence.
- Assertions should verify meaningful outcomes rather than incidental properties.

---

# Required Review Areas

For every test reviewed, evaluate the following categories.

---

## State Mutation

Identify assertions, helper methods or validation logic that:

- update data
- delete data
- acknowledge messages
- complete queue messages
- modify databases
- alter caches
- mutate shared state

while validating outcomes.

Flag as:

> Assertions should not change the state of the system they are observing.

Severity: Critical

---

## Time-Based Synchronisation

Identify patterns such as:

```ts
sleep(...)
wait(...)
setTimeout(...)
delay(...)
```

used as synchronisation mechanisms.

Suggest:

- polling for conditions
- events
- completion signals
- eventual consistency checks

Flag as:

> Waiting is not synchronisation. It is hope with a timeout.

Severity: Critical

---

## Isolation Violations

Identify:

- prerequisite tests
- ordered execution requirements
- cross-test dependencies
- suite-level shared fixtures
- reusable mutable state

Flag as:

> Tests should pass regardless of execution order.

Severity: High

---

## Shared State

Identify usage of:

- shared databases
- shared queues
- shared users
- static identifiers
- shared files
- reused mutable resources

Flag as:

> Shared state is where reliable tests go to die.

Severity: High

---

## Ordering Assumptions

Identify assumptions such as:

```ts
queue[0]
messages[0]
head()
first()
```

where ordering is not explicitly guaranteed by the system.

Flag as:

> The system does not guarantee this ordering.

Severity: High

---

## Behaviour vs Implementation

Identify tests coupled to:

- private state
- internal methods
- object structure
- implementation details
- specific execution paths

Suggest behaviour-focused alternatives.

Flag as:

> The more a test knows about internals, the more fragile it becomes.

Severity: Medium

---

## Environmental Dependencies

Identify:

- timezone assumptions
- locale assumptions
- use of real clocks
- machine-specific paths
- external system dependencies
- network timing assumptions

Flag as:

> A test should control what it depends on.

Severity: High

---

## Weak Assertions

Identify:

```ts
expect(x).toBeTruthy()
expect(x).toBeDefined()
expect(response).toExist()
```

where stronger behavioural assertions are possible.

Flag as:

> Weak assertions create false confidence.

Severity: Medium

---

## Test Data Quality

Identify:

- magic values
- unclear fixtures
- random strings without meaning
- excessively large fixtures
- generated data that hides intent

Flag as:

> Test data should explain the scenario.

Severity: Medium

---

## Flakiness Indicators

Identify:

- retries used to stabilise tests
- timeout increases as fixes
- ignoreFailure patterns
- rerun-until-pass behaviour
- comments such as:

```ts
// flaky
// occasionally fails
// CI issue
// increase timeout if needed
```

Flag as:

> If a test requires special treatment to pass, the problem is the test, not the pipeline.

Severity: High

---

# Severity Levels

## Critical

The test may actively lie, interfere with outcomes or behave non-deterministically.

Examples:

- state-changing assertions
- hard waits used for synchronisation

## High

Likely to create flaky or unreliable tests.

Examples:

- shared state
- ordering assumptions
- cross-test dependencies

## Medium

Maintains confidence but introduces maintenance or readability risks.

Examples:

- weak assertions
- poor test data
- implementation coupling

## Low

Minor maintainability concerns.

---

# Output Format

## Summary

### Confidence Assessment

Choose one:

- High Confidence
- Moderate Confidence
- Low Confidence
- False Confidence

Provide a brief explanation.

---
## Context Assessment

Before producing findings, identify:

### Inferred Test Type

One of:

- Unit Test
- Component Test
- Integration Test
- Contract Test
- End-to-End Test
- Performance Test
- Unknown

### Confidence

High | Medium | Low

### Reasoning

Brief explanation of why this classification was chosen.

If classification is uncertain, continue the review using generic testing principles.

## Findings

For each finding:

### Finding

Describe the issue.

### Severity

Critical | High | Medium | Low

### Principle Violated

Quote the relevant testing principle.

### Why It Matters

Explain the practical impact.

### Suggested Improvement

Provide a concrete example or alternative.

---

## Root Cause Pattern

Identify the dominant category:

- Observability
- Synchronisation
- Isolation
- State Management
- Test Design
- Maintainability

Explain why this category appears repeatedly.

---

## Teaching Moment

Finish every review with:

> If this test failed at 2am during an incident, would an engineer trust the result?

If the answer is no, explain why.

If the answer is yes, explain what properties make the test trustworthy.

## False Confidence Detection

Identify tests that can pass while failing to verify meaningful behaviour.

Examples:

- assertions that always pass
- overly broad assertions
- mocked behaviour identical to implementation
- verification of setup rather than outcome
- checks that prove execution rather than correctness

Flag as:

> A passing test is only valuable if it can fail for the right reason.