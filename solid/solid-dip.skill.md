---
name: "solid-dip"
title: "Dependency Inversion Principle Reviewer"
description: "Reviews whether high-level code depends on contracts rather than concrete implementation choices."
version: "draft-2-agent-ready"
type: "agent-skill"
use_when: "Use when code creates, imports, configures, or depends on services, SDKs, clients, loggers, repositories, providers, packages, or infrastructure."
activation: "Load when the user request matches use_when, or when another SOLID instruction file lists this file as a dependency."
depends_on:
  - "solid-core"
  - "solid-lsp"
  - "solid-isp"
agent_consumption_notes:
  - "Read the metadata first, then follow the instructions as operating context."
  - "Treat imperative instructions as behaviour requirements for the agent."
  - "Do not describe this file as a third-party capability during normal user responses."
  - "Use the user's requested scope as the boundary for action unless the user explicitly expands it."
---

# Dependency Inversion Principle Reviewer

## Name

solid-dip

## Description

Reviews whether high-level code depends on contracts rather than concrete implementation choices.

## When To Use

Use when code creates, imports, configures, or depends on services, SDKs, clients, loggers, repositories, providers, packages, or infrastructure.

## Dependencies

solid-core, solid-lsp, solid-isp

## Agent Consumption Contract

- Use these instructions as active operating context, not as passive documentation.
- Follow direct instructions without referring to the instruction file in user-facing output.
- Preserve the user's scope and ask before extending work beyond that scope.
- Prefer practical, evidence-based action over theoretical explanation unless the user asks to learn.
- If instructions conflict, prefer the option that reduces bugs, maintenance cost, coupling, and future change fragility.

---

## Purpose

Apply the Dependency Inversion Principle (DIP) as a practical design, review, and refactoring framework.

Dependency Inversion is not really about dependencies.

It is about speeding up change when change happens.

High-level code should describe what it needs. It should not decide exactly how that need is fulfilled.

Use DIP to keep systems flexible, testable, swappable, and resilient when concrete implementations change.

Apply DIP anywhere one part of a system needs another part in order to work:

- services
- controllers
- repositories
- adapters
- clients
- providers
- loggers
- printers
- validators
- factories
- React components
- Express middleware
- configuration providers
- external API clients
- NPM packages
- infrastructure code
- domain services
- application services

Treat a dependency as any code, package, module, object, class, service, platform, system, or runtime behaviour that another part of the system needs in order to function.

---

## Core Philosophy

Do not confuse Dependency Inversion with Dependency Injection.

Dependency Inversion is the design principle.

Dependency Injection is one mechanism that can help implement it.

Dependency Inversion asks:

```text
Can high-level code depend on the idea of what it needs rather than the concrete thing that happens to provide it today?
```

Dependency Injection asks:

```text
Can the correct concrete implementation be provided from outside rather than created manually inside the consumer?
```

You usually need both for the full benefit, but they are not the same thing.

---

## Plain-English Definition

Do not build high-level behaviour on concrete implementation details.

Depend on the contract.

Let the system provide the concrete implementation.

A consumer should say:

```text
I need something that can do this job.
```

It should not say:

```text
I need this exact class, from this exact package, created in this exact way, at this exact point.
```

The more exact the dependency, the more expensive future change becomes.

---

## Dependency Inversion vs Dependency Injection

### Dependency Inversion

Dependency Inversion makes implementations interchangeable.

It protects high-level policy from low-level details.

It allows code to depend on abstractions, interfaces, contracts, capabilities, or roles instead of concrete implementations.

Example:

```text
PrinterManager depends on Printer
not PdfPrinter, EpsonPrinter, or BrotherPrinter.
```

---

### Dependency Injection

Dependency Injection provides the chosen implementation from outside the consumer.

It removes the consumer's responsibility to create the concrete dependency.

Example:

```text
PrinterManager receives printers
instead of constructing them itself.
```

---

### Relationship

Dependency Inversion creates the interchangeable contract.

Dependency Injection delivers the interchangeable implementation.

If dependencies are not interchangeable, injection only moves the problem somewhere else.

If dependencies are interchangeable but are still manually constructed inside consumers, the consumer is still coupled to concrete creation.

Use both deliberately.

---

## Primary Diagnostic: The New Keyword Test

The fastest DIP smell is concrete creation inside code whose job is not creation.

Look for:

```ts
new ConcreteClass()
```

inside:

- business logic
- application services
- controllers
- managers
- orchestrators
- validators
- domain logic
- test setup outside the system under test

Ask:

```text
Is this unit responsible for creating this dependency?
```

If no, investigate for DIP violation.

The problem is not the keyword itself.

The problem is concrete decision-making in the wrong place.

---

## Deeper Diagnostic: Independent Change Test

Concrete creation is dangerous when the created thing can change for reasons outside the creating code.

Ask:

```text
Can this dependency change independently of the code that creates it?
```

If yes, the dependency should usually be inverted and supplied externally.

A dependency may be acceptable to create internally when:

- it is a private implementation detail
- it is only used in one place
- it cannot reasonably change independently
- changing it would require changing the enclosing implementation anyway
- it is created and disposed of entirely inside the same atomic implementation
- it does not leak into public contracts or consumer behaviour

Example:

A `BrotherPrinter` implementation may create a private proprietary encoding helper internally if that helper only exists to support the Brother printer implementation and has no independent reason to change outside that printer implementation.

Do not turn every tiny implementation detail into an injected dependency.

Do invert dependencies that introduce external change pressure.

---

## Core Mental Model

Every concrete dependency is a declaration:

```text
Without this exact thing, I cannot work.
```

Every manual construction is a commitment:

```text
I will depend on this concrete implementation until somebody changes this code.
```

Do not make that commitment accidentally.

---

## The Blueprint Test

High-level code should work from blueprints, not hand-placed blocks.

The consumer should describe the shape of what it needs.

The system should provide the concrete material.

A builder should not care whether the supplied block is wood, stone, brick, or obsidian if the blueprint only needs a block-shaped material.

Ask:

```text
Is this code building from a contract, or hand-placing concrete blocks?
```

If the code hand-picks concrete implementations, it is harder to change.

If the code depends on contracts, the implementation can be swapped when requirements change.

---

## Printer Manager Example

Given:

```ts
class PrinterManager {
  private pdfPrinter = new PdfPrinter();
  private epsonPrinter = new EpsonPrinter();
  private brotherPrinter = new BrotherPrinter();
}
```

Investigate immediately.

Questions:

```text
What happens if the customer does not have a Brother printer?
```

```text
What happens when a new printer brand appears?
```

```text
What happens when one printer is offline?
```

```text
What happens when the PDF implementation changes?
```

```text
Does PrinterManager manage printers, or does it create and micromanage printer implementations?
```

A printer manager should usually depend on a collection of printer contracts:

```ts
interface Printer {
  print(document: PrintableDocument): Promise<PrintResult>;
}
```

Then receive implementations from outside:

```ts
class PrinterManager {
  constructor(private readonly printers: Printer[]) {}
}
```

The manager should manage printers.

It should not be permanently shackled to every printer implementation the industry invents.

---

## Logger Example

Do not avoid an interface only because there is one implementation today.

Given:

```text
There is only one logger.
```

Ask:

```text
Only one logger today, or only one logger forever?
```

Logging commonly changes across environments and organisations:

- console logging
- Application Insights
- cloud provider logging
- government department logging stacks
- audit databases
- compliance reporting
- fraud detection pipelines
- structured logs
- flat files

A logger abstraction protects the application from concrete logging infrastructure.

The purpose is not to predict every future logger.

The purpose is to avoid hard-coding today's logger into every part of the system.

---

## High-Level Policy Guidance

High-level policy should not depend on concrete low-level details.

A high-level policy should depend on the interface, role, contract, or capability it needs.

Concrete creation belongs at the edges of the system, composition root, dependency container, framework bootstrap, factory, or adapter boundary.

Ask:

```text
Is this high-level behaviour describing what it needs, or deciding exactly how it gets it?
```

If high-level behaviour chooses concrete implementations directly, the dependency direction is probably wrong.

---

## Red-Flag Phrases

Treat these phrases as prompts for review:

```text
It's only one dependency.
```

```text
I'll just instantiate it here.
```

```text
It's easier this way.
```

```text
It will never change.
```

```text
The ticket only needs this one implementation.
```

```text
We can refactor it later.
```

The most dangerous phrase is:

```text
It will never change.
```

Most systems change sooner than expected.

If a dependency is likely to change, or if the system is expected to evolve with multiple teams, do not hard-code the concrete implementation into business logic.

---

## Common Anti-Patterns

### Anti-Pattern 1: Moving the New Keyword

This is not enough:

```ts
const logger = new Logger();
const service = new UserService(logger);
```

If the concrete dependency is still chosen manually by nearby application code, ownership may only have moved location.

Ask:

```text
Has dependency creation moved to the correct composition boundary, or just moved one file up?
```

---

### Anti-Pattern 2: Dependency Injection as Version Control

Do not use dependency injection to manage many incompatible versions of a domain object.

If versioned implementations are not actually substitutable, injection becomes a mechanism for hiding architectural damage.

Ask:

```text
Are these implementations genuinely interchangeable, or are we using injection to route around incompatible designs?
```

---

### Anti-Pattern 3: Concrete Service Locator Disguised as DI

Be suspicious of global registries, hand-rolled containers, or managers that hide concrete creation behind vague access methods.

Examples:

```text
AppServices.getLogger()
DependencyManager.getPaymentProvider()
GlobalContainer.resolveConcreteThing()
```

Ask:

```text
Is this improving dependency direction, or hiding concrete dependencies behind a different name?
```

---

### Anti-Pattern 4: Factory Everywhere

Factories are useful when object creation is genuinely part of the design.

Factories are harmful when they become a workaround for not understanding dependency injection.

Ask:

```text
Is object creation the responsibility here, or is this factory just a new keyword warehouse?
```

---

### Anti-Pattern 5: Abstracting Without Swappability

Do not create an interface that has no meaningful substitution value and does not protect against change.

Ask:

```text
What concrete change does this abstraction make easier?
```

If no credible answer exists, investigate for overengineering.

---

## Node and TypeScript Guidance

JavaScript and TypeScript projects often violate DIP because the ecosystem does not strongly guide developers towards dependency inversion.

Common smells:

- concrete classes created directly inside Express handlers
- service instances created inside controllers
- SDK clients instantiated inside business logic
- environment variables read from deep inside application code
- config modules imported everywhere as globals
- NPM packages called directly throughout the codebase
- hand-rolled dependency containers with unclear lifecycle rules
- factories used as disguised global constructors
- React components importing infrastructure directly

Ask:

```text
Is this code depending on a contract, or on a concrete package/class/module/environment detail?
```

---

## Express Guidance

Express handlers should not become the place where dependencies are constructed.

Be suspicious of:

```ts
app.post('/users', async (req, res) => {
  const repo = new UserRepository();
  const mailer = new SendGridMailer();
  const service = new UserService(repo, mailer);
  await service.create(req.body);
});
```

The handler now knows too much about concrete infrastructure.

Prefer composition outside the route handler and pass in the application service or handler dependencies from the boundary.

---

## React Guidance

React components should not depend directly on infrastructure when a narrower contract would do.

Be suspicious when components:

- instantiate API clients
- import concrete service singletons
- read environment configuration directly
- know concrete analytics/logging providers
- manually construct dependencies used by child components

Ask:

```text
Can this component receive behaviour through props, hooks, context, or an adapter contract instead of constructing infrastructure directly?
```

Do not use context as a dumping ground for global concretes.

---

## Config and Environment Guidance

Environment variables and configuration are dependencies.

Do not read environment variables from everywhere.

Centralise configuration at the boundary and expose a contract for the parts of the system that need it.

Ask:

```text
Does this code need configuration, or does it need a specific environment variable name?
```

High-level code should not depend on environment key strings, cloud-specific configuration shape, or deployment-specific details.

---

## NPM Package Guidance

An NPM package can be a concrete dependency.

Do not scatter direct package usage across the system when package behaviour is important to the domain or likely to change.

Examples:

- HTTP clients
- logging libraries
- payment SDKs
- analytics SDKs
- date/time libraries
- feature flag clients
- cloud provider SDKs

Wrap important external packages behind contracts or adapters when change, testing, or substitution matters.

Do not wrap every tiny utility package automatically.

Use judgement.

---

## Testing Heuristics

DIP problems often show up as painful tests.

### Mocking Difficulty

If a unit test cannot easily replace dependencies with mocks, fakes, or stubs, investigate dependency direction.

Ask:

```text
Can every external collaborator be replaced without changing the system under test?
```

If no, the code may depend on concrete implementations in the wrong place.

---

### Concrete Construction in Tests

Inside a unit test, concrete creation should usually be limited to the system under test and simple data objects.

Be suspicious when a unit test must create:

- real repositories
- real SDK clients
- real network clients
- real loggers
- real config systems
- real databases
- real framework infrastructure

Ask:

```text
Why does this test need the real thing?
```

---

### Testing the Mock

If tests become focused on proving that the mock is configured correctly, the system may not have clean dependency boundaries.

Ask:

```text
Is this test verifying behaviour, or fighting concrete coupling?
```

---

### Integration Test Boundary

Integration tests may intentionally use concrete dependencies.

Even then, keep the boundary clear.

If testing two components together, avoid accidentally pulling in the whole world.

---

## When Not To Apply DIP

Do not apply DIP dogmatically.

DIP may be unnecessary for:

- throwaway scripts
- toy projects
- one-off tools
- very small single-developer utilities
- prototypes intended to be discarded
- tiny front-end-only tools with no meaningful interchangeable components
- tightly scoped algorithms with no realistic implementation variation
- code where abstraction would make the design harder to understand than the concrete implementation

Example:

A personal MathHammer app that performs fixed tabletop dice calculations may not need dependency injection if:

- one person maintains it
- the domain is tiny
- there are no swappable infrastructure components
- the algorithm is stable
- the app is not enterprise scale
- the cost of abstraction outweighs the benefit

Use DIP when change, testing, scale, collaboration, or substitution justify it.

Do not use DIP as architecture theatre.

---

## Enterprise and Government Scale Guidance

DIP becomes more valuable as scale increases.

Apply stronger DIP discipline when:

- multiple teams work on the same system
- external systems change independently
- cloud providers or platform services may change
- government departments require different implementation details
- logging, auditing, compliance, or reporting requirements vary
- releases need to continue while infrastructure changes
- long-lived services must evolve safely
- tests need reliable isolation

At scale, hard-coded dependencies create coordination cost.

Inverted dependencies reduce the need for every team to understand every concrete detail.

---

## Change-Speed Heuristic

DIP is working when a major implementation can be swapped with minimal disruption.

Ask:

```text
If this concrete implementation changed tomorrow, how many files would need to change?
```

If the answer is many, dependency direction may be wrong.

Then ask:

```text
Could we introduce the replacement behind the same contract and change only composition?
```

If yes, the design is moving in the right direction.

---

## Practical Review Process

When reviewing dependencies:

1. Identify the high-level behaviour.
2. Identify every concrete dependency it creates or imports.
3. Decide whether each dependency can change independently.
4. Decide whether the high-level behaviour should know that concrete detail.
5. Check whether a suitable abstraction already exists.
6. Check whether the abstraction is genuinely swappable.
7. Check whether dependency creation happens at an appropriate boundary.
8. Check whether tests can replace the dependency fluently.
9. Challenge claims that the dependency will never change.
10. Recommend inversion, injection, adapter, factory, or direct use depending on the situation.

Do not blindly wrap everything.

Invert dependencies where doing so protects change, tests, or understanding.

---

## AI-Generated Code Review

When reviewing AI-generated code, assume concrete dependencies may be hard-coded by default.

Check for:

- `new` inside business logic
- concrete SDK clients inside services
- route handlers constructing their own dependencies
- direct environment variable reads deep in the code
- global singleton access
- package APIs scattered across the system
- impossible-to-mock collaborators
- hand-rolled dependency containers
- concrete loggers everywhere
- factories that only hide concrete creation

Require generated code to justify why concrete dependencies belong where they are.

If the implementation may change, test isolation matters, or the dependency is infrastructure, prefer inversion.

---

## Relationship With Other SOLID Principles

DIP ties the other SOLID principles together, but it depends on them being used well.

### SRP

If a unit creates, configures, and uses its collaborators, it may be doing more than one job.

### OCP

If a dependency cannot be swapped without modifying high-level code, extension is harder.

### LSP

Injected implementations must be genuinely substitutable.

Do not inject versions or implementations that do not share a trustworthy contract.

### ISP

Depend on narrow contracts that consumers actually need.

Do not inject giant interfaces full of unrelated operations.

DIP only works well when abstractions are honest, coherent, and swappable.

---

## Success Criteria

DIP is being applied correctly when:

- high-level code depends on contracts rather than concrete implementations
- concrete creation happens at appropriate composition boundaries
- dependencies can be swapped without changing business logic
- tests can replace collaborators fluently
- implementation changes do not cascade through unrelated code
- external packages are wrapped when change or testability requires it
- configuration is centralised and exposed through useful contracts
- injection is not used as version control
- factories are used only when creation is genuinely part of the design
- interfaces provide meaningful substitution value
- small disposable tools are not over-engineered unnecessarily

---

## Core Mental Model

A likely DIP violation exists when:

```text
High-level code decides which concrete implementation it depends on.
```

A deeper DIP violation exists when:

```text
Changing a low-level implementation forces unrelated high-level behaviour to change.
```

The goal is simple:

```text
Let high-level code describe what it needs.
Let the system decide what concrete implementation fulfils that need.
Keep change fast when change eventually happens.
```
