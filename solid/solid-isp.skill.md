---
name: "solid-isp"
title: "Interface Segregation Principle Reviewer"
description: "Reviews whether consumers depend only on coherent contracts they need, without bloated contracts or meaningless fragments."
version: "draft-2-agent-ready"
type: "agent-skill"
use_when: "Use for interfaces, props, DTOs, request/response objects, config, service contracts, modules, packages, APIs, and event schemas."
activation: "Load when the user request matches use_when, or when another SOLID instruction file lists this file as a dependency."
depends_on:
  - "solid-core"
  - "solid-srp"
  - "solid-lsp"
agent_consumption_notes:
  - "Read the metadata first, then follow the instructions as operating context."
  - "Treat imperative instructions as behaviour requirements for the agent."
  - "Do not describe this file as a third-party capability during normal user responses."
  - "Use the user's requested scope as the boundary for action unless the user explicitly expands it."
---

# Interface Segregation Principle Reviewer

## Name

solid-isp

## Description

Reviews whether consumers depend only on coherent contracts they need, without bloated contracts or meaningless fragments.

## When To Use

Use for interfaces, props, DTOs, request/response objects, config, service contracts, modules, packages, APIs, and event schemas.

## Dependencies

solid-core, solid-srp, solid-lsp

## Agent Consumption Contract

- Use these instructions as active operating context, not as passive documentation.
- Follow direct instructions without referring to the instruction file in user-facing output.
- Preserve the user's scope and ask before extending work beyond that scope.
- Prefer practical, evidence-based action over theoretical explanation unless the user asks to learn.
- If instructions conflict, prefer the option that reduces bugs, maintenance cost, coupling, and future change fragility.

---

## Purpose

Apply the Interface Segregation Principle (ISP) as a practical design, review, and refactoring framework.

ISP is not really about interfaces.

It is about crystallising the domain into reusable, hot-swappable parts that are atomic enough to protect against change.

Use ISP to make sure consumers depend only on the coherent contract they actually need, while avoiding both oversized catch-all interfaces and meaningless clouds of tiny fragments.

Apply ISP anywhere one part of a system exposes a contract to another part:

- TypeScript interfaces
- C# interfaces
- Java interfaces
- React props
- Vue props
- service contracts
- DTOs
- request objects
- response objects
- config objects
- repository contracts
- validator contracts
- component APIs
- module boundaries
- package APIs
- HTTP APIs
- event schemas
- domain models

Treat an interface as any boundary between supplying code and consuming code.

---

## Core Philosophy

Do not reduce ISP to:

```text
Make interfaces as small as possible.
```

That is wrong.

Do not reduce ISP to:

```text
Put everything useful into one big interface.
```

That is also wrong.

Create interfaces that match the thing they are supposed to explain, represent, or protect.

A good interface describes one coherent concept at the right level of atomicity.

It contains no more than the concept needs.

It contains no less than the concept needs.

---

## Plain-English Definition

A consumer should only know about what it needs to know about.

More precisely:

```text
A consumer should depend on the smallest coherent contract required to do its job.
```

The word `coherent` matters.

The goal is not microscopic interfaces.

The goal is useful atomic parts that can be composed into larger structures without forcing consumers to depend on unrelated behaviour, data, or domain concepts.

---

## Primary Diagnostic: Consumer Knowledge Test

Ask:

```text
What does this consumer actually need to know?
```

Then check the interface used by that consumer.

A likely ISP violation exists when the consumer is forced to depend on:

- methods it does not call
- properties it does not use
- concepts outside its responsibility
- domain details it should not understand
- lifecycle operations it does not participate in
- write operations when it only reads
- read operations when it only writes
- admin operations when it only performs user work
- reporting operations when it only performs transactional work

If the consumer only needs an item name, do not force it to depend on the whole item, product, stock, pricing, supplier, audit, and fulfilment model.

---

## Secondary Diagnostic: Interface Drift Test

An interface drifts when it starts describing a different concern from the one it was created to represent.

Ask:

```text
Has this interface started explaining a different thing?
```

Example:

A `Saxophone` interface should describe what makes something a saxophone.

If it starts including manufacturing-material restrictions, it may have drifted into a factory or manufacturing concern.

Questions:

```text
Does this member describe the concept itself?
```

```text
Or does this member describe how the concept is created, stored, displayed, reported, audited, exported, or transported?
```

If the interface starts switching concern, split or relocate the contract.

---

## Semantic Duplication Test

Do not only compare names.

Compare meaning.

A likely ISP issue exists when multiple interfaces contain semantically identical concepts but expose them through different contracts.

Examples:

- two different repository read interfaces that do the same job
- multiple validator interfaces that validate the same base concept
- several React prop shapes that all describe the same display item
- DTOs duplicating the same domain concept under different names
- service contracts repeating the same operation with different prefixes or suffixes

Ask:

```text
Are these members semantically the same thing?
```

Two members with the same name may mean different things.

Two members with different names may mean the same thing.

Do not create a new interface when an existing interface already represents the same contract.

Segregation is not duplication.

---

## Atomicity and Composition

Good ISP creates useful building blocks.

Think in Lego rather than dust or concrete.

Bad interface design creates either:

```text
one giant glued-together lump
```

or:

```text
tiny fragments so small nobody can build with them
```

Good interface design creates atomic pieces that compose into meaningful structures.

A useful atomic interface is:

- understandable
- reusable
- independently testable
- semantically coherent
- stable under likely change
- easy to compose with other interfaces

Avoid splitting interfaces so aggressively that developers cannot understand how to assemble them.

Avoid merging interfaces so aggressively that consumers are forced to depend on unrelated concepts.

---

## Size Is Evidence, Not Proof

Large interfaces are not automatically wrong.

Tiny interfaces are not automatically right.

A large interface may be correct when the domain concept genuinely requires high resolution.

Examples:

- legally defined plant models
- legally defined animal models
- complex biosecurity concepts
- regulated domain records
- rich domain-specific repository contracts

A small interface may be wrong when it fragments one coherent concept into meaningless pieces.

Ask:

```text
Does the size match the domain concept and expected change pressure?
```

Do not judge ISP by counting methods alone.

Judge by responsibility, coherence, consumer need, and change impact.

---

## Change Analysis

ISP is a change-management principle.

Ask:

```text
Where will this change?
```

```text
Why would this change?
```

```text
Who would need this change?
```

```text
Which consumers should be protected from this change?
```

```text
Is this concept local, shared, generic, domain-specific, or specialised?
```

Move contract members up or down depending on the level where change actually belongs.

If every consumer needs the same concept, it may belong in a shared interface.

If only some consumers need it, segregate it into a more specific interface.

If only one implementation needs it, do not force it into the shared contract.

---

## Relationship With Completeness

An interface should fully describe the concept it represents.

It should not under-describe the concept so badly that developers create endless duplicate extensions.

It should not over-describe the concept so badly that consumers are forced to understand unrelated behaviour.

Ask:

```text
Is this interface complete for its current responsibility?
```

```text
Has this interface absorbed responsibilities from neighbouring concepts?
```

---

## Service Interface Guidance

Service interfaces often become dumping grounds.

Be suspicious when a service interface contains repeated verbs with different suffixes or prefixes.

Example smell:

```text
generateUserAuditReport
generateItemOrderReport
generateBasketOrderReport
generateOrderSalesReport
generateLogReport
```

This may indicate a missing generic report contract, a missing reporting service, or poor domain decomposition.

Ask:

```text
Is this interface describing one service, or many services wearing the same name?
```

---

## Example: User Service

Given:

```ts
interface IUserService {
  createUser(): void;
  updateUser(): void;
  deleteUser(): void;
  resetPassword(): void;
  sendEmail(): void;
  exportUsers(): void;
  generateAuditReport(): void;
}
```

Investigate immediately.

The first three methods may belong to user lifecycle management.

The later methods raise questions:

```text
Does password reset belong to user management, authentication, or account security?
```

```text
Why does a user service send email?
```

```text
Is exporting users a reporting/export concern?
```

```text
Is generating audit reports an audit/reporting concern?
```

Do not treat association with users as ownership by user service.

Related data is not the same as responsibility ownership.

---

## Repository Contract Guidance

Repository interfaces often benefit from atomic segregation.

A useful repository design may separate:

- read
- write
- update
- delete

This allows consumers to depend only on the capability they need.

Example:

```text
IReadRepository<T>
IWriteRepository<T>
IUpdateRepository<T>
IDeleteRepository<T>
```

A consumer that only reads should not depend on write or delete operations.

A consumer that only writes should not depend on read or delete operations.

However, do not duplicate identical repository interfaces under different names.

If two interfaces are structurally and semantically identical, reuse the existing contract.

---

## React Props Guidance

React props are interfaces.

Treat component props as public contracts between caller and component.

A component prop shape should describe what the component needs, not whatever object the caller happens to have.

Example:

A basket item component may need:

- display name
- image URL
- quantity
- price display value
- quantity change handler
- remove handler

It probably does not need the full product, stock, supplier, fulfilment, audit, and pricing tree.

Ask:

```text
What does this component actually need to render and behave correctly?
```

If catalogue item, basket item, and order item props repeat the same display contract, consider a shared display-item interface.

Do not create three unrelated prop shapes for the same semantic concept unless the meanings genuinely differ.

---

## Context Provider Guidance

Context providers are not automatically ISP issues.

They may reveal wider design problems in the component tree.

When reviewing context providers, ask:

```text
Is this context exposing more knowledge than consumers need?
```

```text
Are consumers depending on a large context object when they only need one small capability?
```

```text
Is context being used to hide poor component decomposition?
```

If context consumers repeatedly select tiny pieces from a large provider, consider segregating provider contracts or introducing narrower hooks.

---

## DTO, Request, Response, and Config Guidance

DTOs, request objects, response objects, and config objects are contracts.

Keep them dumb, but do not let them become meaningless dumping grounds.

Ask:

```text
Does this object describe one transfer boundary or many unrelated concerns?
```

```text
Are consumers forced to receive fields they should not know about?
```

```text
Are unrelated configuration areas mixed into one object?
```

```text
Would changing one section force unrelated consumers to recompile, retest, or update?
```

Super objects are not always ISP problems, but they often increase human misunderstanding, accidental coupling, and change drag.

---

## Test Smells

ISP problems often show up in tests.

### Coverage Without Meaning

If an interface member can be covered by a meaningless existence test, ask whether the member carries useful behaviour or belongs elsewhere.

Example smell:

```text
assert that property exists
```

with no meaningful behavioural consequence.

---

### Values Do Not Matter

If changing input values makes no difference to expectations, the test may reveal unused or irrelevant contract members.

Ask:

```text
Does this consumer actually care about this member?
```

---

### Copied Test Suites

If multiple implementations or interfaces have copied-and-pasted tests for the same behaviour, investigate whether a shared segregated interface is missing.

Shared behaviour should often be tested once as a reusable contract and applied to every implementation.

---

### Untested Interface Regions

If parts of an interface are never meaningfully tested through a consumer, those parts may not belong to the consumer-facing contract.

Do not trust coverage percentages alone.

High coverage can still hide bad interface boundaries.

---

## Implementation Usage Review

Do not judge ISP from a single interface file alone.

Inspect:

- implementations
- consumers
- neighbouring interfaces
- duplicated concepts
- adapters
- call sites
- tests
- change history where available

Ask:

```text
Which methods are always used together?
```

```text
Which methods are never used together?
```

```text
Which consumers only need a subset?
```

```text
Which concepts repeat across multiple contracts?
```

```text
Which changes would affect unrelated consumers?
```

ISP requires system awareness.

---

## Generic Interfaces

If something can be made generic without obscuring domain meaning, consider making it generic.

Generic contracts can prevent repeated, nearly identical interfaces.

Example:

```text
IValidator<T>
```

may be better than:

```text
IUserValidator
ICustomerValidator
IAccountValidator
```

when the validation contract is semantically the same.

Do not use generics to hide different meanings behind one shape.

Generic reuse is good only when the contract is genuinely shared.

---

## Pragmatic Review Tiers

Use three practical outcomes.

### Acceptable For Now

Use when the design is not ideal but does not create dangerous coupling.

Examples:

- slightly over-segregated interfaces
- mildly awkward but understandable composition
- interface shape that can be improved later without contaminating the domain

Document the concern if useful.

---

### Track As Technical Debt

Use when the design creates maintenance drag but does not immediately corrupt the domain model.

Examples:

- large interface used by several consumers that only need subsets
- duplicated interface contracts
- repeated props or DTO shapes that are semantically the same
- service interface beginning to collect unrelated operations

Create a clear technical debt item with a proposed segregation or consolidation plan.

Do not bury the issue.

---

### Hard Stop

Use when the interface blurs domain boundaries in a way that future work will build on top of.

Examples:

- bird and mammal concepts merged into one misleading contract
- user lifecycle mixed with payment, audit, reporting, and export behaviour
- plant and animal domain concepts blended without a shared biological abstraction
- contract shape that forces many consumers to depend on unrelated domain knowledge
- interface mistake likely to be sewn into multiple implementations and adapters

Do not approve the design without correction or explicit architectural decision.

---

## AI-Generated Code Review

When reviewing AI-generated code, assume interface boundaries may be superficial.

Check for:

- oversized service interfaces
- duplicated interfaces with different names
- React props using entire domain objects unnecessarily
- DTOs carrying unrelated data
- request objects used as internal domain models
- config objects mixing unrelated areas
- consumers depending on methods they do not call
- repeated semantic concepts across multiple contracts
- generic opportunities missed
- tiny interfaces that cannot be usefully composed

Require the generated code to justify interface boundaries through consumer need and change impact.

---

## Relationship With Other SOLID Principles

ISP overlaps heavily with the other SOLID principles.

### SRP

If an interface describes multiple responsibilities, SRP is probably involved.

### OCP

If new behaviour keeps being bolted onto an existing interface for only a subset of consumers, OCP is probably involved.

### LSP

If implementations fake or ignore interface members, LSP is probably involved.

### DIP

If high-level code depends on broad concrete contracts instead of narrow abstractions, DIP is probably involved.

Do not diagnose every interface smell as ISP alone.

Use ISP to ask whether the contract boundary protects consumers from unnecessary knowledge and unnecessary change.

---

## Success Criteria

ISP is being applied correctly when:

- consumers depend only on coherent contracts they actually need
- interfaces represent domain concepts accurately
- large interfaces are justified by domain complexity
- small interfaces remain meaningful and composable
- shared semantic concepts are not duplicated under different names
- unrelated domain concepts do not drift into the same contract
- React props describe component needs rather than caller convenience
- service interfaces avoid becoming dumping grounds
- repositories expose only required capabilities
- DTOs and request objects do not become accidental domain models
- tests can verify shared contracts without copy-paste duplication
- future changes remain localised to the right conceptual level

---

## Core Mental Model

A likely ISP violation exists when:

```text
A consumer must depend on knowledge, methods, data, or concepts it does not need to perform its job.
```

A deeper ISP violation exists when:

```text
An interface no longer cleanly represents one coherent domain concept at the level where change actually occurs.
```

The goal is not small interfaces.

The goal is accurate, atomic, reusable, hot-swappable contracts that protect the system from unnecessary change.
