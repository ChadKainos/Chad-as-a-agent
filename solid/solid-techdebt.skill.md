---
name: "solid-techdebt"
title: "SOLID Technical Debt Handler"
description: "Defines when SOLID compromise can be accepted, classified, recorded, and kept visible."
version: "draft-2-agent-ready"
type: "agent-skill"
use_when: "Use when a SOLID issue is real but cannot be fixed safely within current scope, delivery constraints, or ownership boundaries."
activation: "Load when the user request matches use_when, or when another SOLID instruction file lists this file as a dependency."
depends_on:
  - "solid-core"
  - "solid-review"
  - "solid-refactor"
agent_consumption_notes:
  - "Read the metadata first, then follow the instructions as operating context."
  - "Treat imperative instructions as behaviour requirements for the agent."
  - "Do not describe this file as a third-party capability during normal user responses."
  - "Use the user's requested scope as the boundary for action unless the user explicitly expands it."
---

# SOLID Technical Debt Handler

## Name

solid-techdebt

## Description

Defines when SOLID compromise can be accepted, classified, recorded, and kept visible.

## When To Use

Use when a SOLID issue is real but cannot be fixed safely within current scope, delivery constraints, or ownership boundaries.

## Dependencies

solid-core, solid-review, solid-refactor

## Agent Consumption Contract

- Use these instructions as active operating context, not as passive documentation.
- Follow direct instructions without referring to the instruction file in user-facing output.
- Preserve the user's scope and ask before extending work beyond that scope.
- Prefer practical, evidence-based action over theoretical explanation unless the user asks to learn.
- If instructions conflict, prefer the option that reduces bugs, maintenance cost, coupling, and future change fragility.

---

## Purpose

Apply transparent, pragmatic SOLID technical debt handling when a SOLID issue cannot be fixed safely inside the user's current scope.

SOLID technical debt is not just bad code.

It is a deliberate compromise that knowingly makes a system worse in order to provide stakeholder value sooner at a reduced implementation quality level.

That value erodes towards zero over time as technical debt accumulates. Once the value has been consumed, the debt begins to negatively affect other parts of the system. The speed and scale of that impact depends on the severity of the debt.

Use this guidance to keep compromise visible, explicit, reviewed, and recoverable.

Do not use technical debt as a hiding place for lazy design.

---

## Core Philosophy

Technical debt is acceptable only when the compromise is conscious, reasoned, and visible.

A hidden compromise is not managed debt.

A hidden compromise is damage.

A SOLID debt item should make the future cost clear enough that the team wants to remove it rather than forget it.

Be direct, evidence-based, and technically explicit.

Do not blame individuals.

Do not soften the technical truth into corporate fog.

---

## Prime Directive

Be responsive and pragmatic to the user's needs without compromising long-term quality or transparency of code quality.

If the debt cannot be removed now, expose it clearly.

If the debt can be removed within the user's scope and a viable effort level, remove it instead of creating debt.

---

## When To Create SOLID Technical Debt

Create or recommend SOLID technical debt when a SOLID issue is understood but cannot reasonably be fixed now.

Valid triggers include:

- delivery pressure
- business deadline
- issue outside the user's current scope
- issue outside the agent's authorised scope
- architectural approval required
- shared contract change required
- legacy code too risky to alter immediately
- complexity has expanded beyond the story's expected scope
- risk has expanded beyond the story's expected scope
- effort has expanded beyond the story's story-point assumptions
- fixing now would require ticket re-evaluation
- fixing now would require agreement from one level above the author or wider team

Every debt item needs a clear reason why the problem cannot be tackled now when the problem is visible in the user and agent context.

With AI agents, the reasoning must be especially sound because a well-used agent can reduce effort. Do not create debt merely because the agent or developer did not want to perform the work.

---

## When Not To Create Debt

Do not create a technical debt item when the issue can be fixed within the requested scope and viable effort.

Fix it.

Do not create debt as a substitute for doing the engineering work.

Do not create debt when the compromise creates a known unsafe path that future work must immediately build on top of.

---

## Hard Stops

Do not accept debt when it locks the codebase into a hack.

Hard stop examples:

- the debt forces future work to build more hacks immediately
- the debt is hard-coded into a core feature
- the debt compromises future work before it begins
- the debt introduces known bugs
- the debt introduces known security issues
- the debt can be fixed within current scope and viable time
- the debt pushes the code into a corner where only more hacks are possible
- the debt corrupts a public contract or shared domain boundary
- the debt creates misleading interfaces or substitutes that consumers will trust incorrectly
- the debt creates coupling that downstream work cannot avoid
- the debt hides behaviour that will be relied on by other teams

If a debt item would compromise the future shape of the system, do not treat it as acceptable debt without explicit architectural decision.

---

## Acceptable Debt vs Lazy Compromise

### Acceptable Debt

It is acceptable debt when:

- the compromise has been thought through
- at least one peer has discussed it
- at least one person one level above the author, or an appropriate technical lead, has visibility
- the team agrees the compromise is necessary
- the rationale is written down
- the risk is visible
- the recommended fix is clear
- the debt has a recovery path
- the debt does not lock the codebase into future hacks

### Lazy Compromise

It is lazy compromise when:

- the developer creates a debt item without trying to avoid the compromise
- the team has not discussed the issue
- no peer has reviewed the compromise
- no higher-level technical owner has visibility where needed
- the debt exists only because the developer wanted to move on
- the debt can be fixed within scope
- the debt hides the issue instead of exposing it
- the debt has no clear recovery path

Do not dress lazy compromise as technical debt.

---

## Required Debt Ticket Content

Every SOLID technical debt ticket must contain:

- clear title
- affected file paths
- affected functions/classes/modules/services
- copied offending code snippet or enough code context to identify the issue
- affected SOLID principle or principles
- technical explanation of the problem
- deep technical walkthrough of the design issue
- explanation of why it was not fixed immediately
- list of known negative effects
- list of likely secondary and third-order effects
- risk level
- downstream consumers or areas likely affected
- recommended fix
- acceptance criteria
- suggested priority
- owner/team if appropriate
- evidence used to justify the ticket

If a separate ticket-creation skill exists, call it or follow its expected ticket structure.

---

## Tone and Accountability

Be very explicit.

The ticket should be clear enough that the team feels motivated to remove the debt.

The ticket should not be cruel, personal, or blame-driven.

Call out roles only when useful and never target non-leadership individuals by name.

If accountability is required, refer to title or role rather than personal details.

Examples:

```text
Approved by technical lead
```

```text
Deferred by delivery decision
```

```text
Requires architecture review
```

Avoid:

```text
<person> wrote bad code
```

```text
<tester> missed this
```

```text
<developer> caused this
```

The shame should attach to the debt existing, not to an individual person.

---

## Priority Levels

Use these priority levels:

- Low
- Medium
- High
- Urgent

Security-related debt is automatically prioritised at the security issue's priority plus one level, because technical debt is often treated as nice-to-have sprint work and needs elevation when security risk is involved.

Do not downplay security debt.

---

## Low Priority

Use Low when the debt is bad practice or a non-expanding local hack.

Characteristics:

- hyper-localised
- unlikely to affect anything else
- no meaningful downstream blast radius
- no known security issue
- no public contract impact
- unlikely to be copied
- unlikely to block future work

Examples:

- slightly awkward local implementation
- small naming/structure issue that does not mislead consumers
- minor over-separation that does not spread
- local workaround that is isolated and documented

Low priority does not mean ignore forever.

It means the debt is contained.

---

## Medium Priority

Use Medium when debt affects more than its own hyper-local area but does not extend beyond the immediate local unit.

Characteristics:

- affects a whole function or immediately surrounding code
- compromises change resilience inside a local unit
- increases bug surface area locally
- increases maintenance surface area locally
- creates low-risk security concern
- causes tests to be more awkward locally
- likely to slow near-term changes in the same function/class

Example:

If debt starts as a statement-level compromise but the whole function is now compromised in change resilience, bug surface area, maintenance surface area, or low-risk security posture, use Medium.

---

## High Priority

Use High when debt affects a wider space such as a class, component, service, small module, local API boundary, or shared implementation area.

Characteristics:

- a local statement compromises class-level design
- a function-level issue compromises module behaviour
- a service interface starts collecting unrelated responsibilities
- concrete dependencies leak into high-level behaviour
- tests require multiple awkward changes for one behaviour
- future changes in the area are likely to be slower or riskier
- duplicated patterns are likely to be copied
- consumers may become coupled to the compromise

Examples:

- class now has multiple reasons to change
- service has started to act as a dumping ground
- interface includes operations only some consumers need
- side effect introduced into a method that consumers assume is pure
- concrete SDK dependency introduced into business logic

---

## Urgent Priority

Use Urgent when debt has clear impact on the wider domain, system, or future work.

Characteristics:

- affects domain boundaries
- affects public contracts
- affects shared packages or libraries
- affects many consumers
- compromises future feature work
- forces future work into more hacks
- creates known bugs
- creates known security issues
- creates architectural lock-in
- undermines release safety
- compromises substitution, extension, or dependency direction at system level

Examples:

- public interface lies about behaviour
- shared contract forces unrelated consumers to change
- dependency injection used to route incompatible versions
- core service hard-codes volatile infrastructure
- domain model blends unrelated concepts
- API contract requires future consumers to handle bad design

Urgent debt should trigger immediate review, re-planning, or escalation.

---

## Code Comments

Add a code comment only when the comment helps keep accepted debt visible.

A useful comment:

- references the technical debt ticket number
- explains why the compromise exists
- points to the intended fix
- is close to the compromised code
- prevents future developers from copying the pattern blindly

Example:

```ts
// TECH DEBT: <ticket-number>
// Temporary SRP compromise: audit publishing is retained here to meet release scope.
// Refactor by moving audit publishing behind AuditPublisher and updating callers.
```

If the ticket does not exist yet, leave a clear placeholder:

```ts
// TECH DEBT: <ticket-number-needed>
// This compromise must be tracked before this pattern is copied.
```

The blank ticket reference should create pressure to create the ticket rather than normalising untracked debt.

Do not add noisy comments for every minor issue.

Do not add comments that blame individuals.

Do not add comments that replace proper tracking.

---

## Debt Outside User Scope

If a debt issue is outside the user's requested scope and does not affect the current work:

1. Complete the user's requested work first.
2. Do not wander away from the requested scope.
3. At the end, call out the observed issue briefly.
4. Advise the user to investigate or allow a deeper assessment.
5. Explain that expanding scope has human-process and token-burn cost.

Suggested wording:

```text
I noticed a possible SOLID debt issue outside the requested change. It does not block this work, so I have not expanded scope. If useful, I can assess it separately and produce a debt report.
```

Be proactive in calling out issues.

Do not hijack the user's request.

---

## Debt Inside User Scope

If the issue is inside the user's requested scope:

- fix it if viable
- create debt only if there is a sound reason it cannot be fixed now
- explain the reason
- expose the risk
- recommend the follow-up

Do not create debt for in-scope issues that can be fixed immediately.

---

## AI Agent Reasoning Standard

Because an agent can often reduce implementation effort, the bar for deferring work is higher.

Before creating debt, check:

```text
Can this be fixed now within scope?
```

```text
Can this be fixed safely using the available context?
```

```text
Would fixing it require touching code outside the user's sphere of influence?
```

```text
Would fixing it require team, product, architecture, or security approval?
```

```text
Has complexity, risk, or effort grown beyond the current ticket assumptions?
```

Only create debt when the reason survives these questions.

---

## SOLID Debt Categories

### SRP Debt

A unit has more than one reason to change.

Common risks:

- hidden side effects
- difficult tests
- higher bug surface area
- change in one concern affects another

### OCP Debt

New behaviour is added in a way that forces unrelated modification or creates speculative extension.

Common risks:

- consumer cascade changes
- inheritance chain hell
- just-in-case extension
- future features require repeated modification

### LSP Debt

A substitute does not keep the same promises as the thing it replaces.

Common risks:

- consumers need special cases
- hidden exceptions
- unexpected side effects
- broken behavioural trust

### ISP Debt

Consumers depend on knowledge, methods, or data they do not need.

Common risks:

- interface drift
- dumping-ground contracts
- duplicated semantic interfaces
- broad change impact

### DIP Debt

High-level code depends on concrete implementations or decides concrete creation directly.

Common risks:

- hard-to-test code
- infrastructure lock-in
- package/vendor coupling
- changes cascade through business logic

---

## Debt Ticket Template

Use this template unless a dedicated ticket creation skill provides a better one.

```markdown
# Technical Debt: <clear technical title>

## Summary
<One or two sentences describing the debt clearly.>

## Affected Area
- Path: `<file path>`
- Function/Class/Module: `<name>`
- Related consumers: `<known consumers>`

## Offending Code
```<language>
<copy of relevant code snippet>
```

## SOLID Principles Affected
- <SRP/OCP/LSP/ISP/DIP>

## Technical Walkthrough
<Explain exactly why this is a SOLID issue. Include how the current code behaves, where the boundary is wrong, and what change pressure exposes the problem.>

## Reason For Deferral
<Explain why this was not fixed immediately. This must be a concrete reason, not convenience.>

## Negative Effects
- <direct effect>
- <secondary effect>
- <third-order effect>

## Risk Level
<Low/Medium/High/Urgent>

## Recommended Fix
<Describe the smallest safe refactor or phased plan.>

## Acceptance Criteria
- <criterion 1>
- <criterion 2>
- <criterion 3>

## Notes
<Any team, approval, architecture, security, or delivery context. Avoid personal blame.>
```

---

## Response Behaviour

When debt is created or recommended:

- be explicit
- be concise where possible
- include evidence
- include code references
- avoid personal blame
- explain risk clearly
- recommend a realistic fix
- state why now is not the right time to fix if applicable

When debt should not be accepted:

- say so clearly
- explain why it is a hard stop
- recommend immediate correction or escalation

---

## Do Not

Do not create debt for work that can be fixed now.

Do not hide debt behind vague wording.

Do not blame individual developers, testers, or non-leadership contributors.

Do not call something technical debt just because the code is ugly.

Do not accept debt that creates known bugs or security issues without escalation.

Do not accept debt that forces future work into more hacks.

Do not wander outside the user's scope to investigate unrelated debt unless asked.

Do not use comments as a replacement for a tracked ticket.

---

## Success Criteria

SOLID technical debt handling is successful when:

- the compromise is deliberate
- the reason for deferral is explicit
- the risk is visible
- the affected code is identified
- the offending code is shown or clearly referenced
- the negative effects are documented
- the recovery path is clear
- priority reflects blast radius and risk
- no individual is blamed unfairly
- the user request remains in scope
- future maintainers can understand why the debt exists
- the team has enough information to remove the debt later

---

## Core Summary

```text
Technical debt is a deliberate compromise, not a hiding place.
```

```text
If it can be fixed now within scope, fix it.
```

```text
If it must be deferred, make it explicit enough that nobody can pretend it is fine.
```

```text
Acceptable debt is discussed, reasoned, visible, and recoverable.
```

```text
Lazy compromise is unreviewed, avoidable, hidden, and future-hostile.
```
