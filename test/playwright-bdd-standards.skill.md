---
name: playwright-bdd-standards
description: 'Playwright BDD test standards for ftts-ui-tests. Use when writing, reviewing, or refactoring Playwright BDD feature files, step definitions, flows, page objects, or test data. Covers feature file structure, tagging, thin steps, BasePage, dynamic page resolution, flow layer, and parallel-safe execution rules.'
argument-hint: 'Describe the page or scenario you want to write or migrate (e.g. "write a feature file for the Candidate Details page")'
---

# Playwright BDD Test Standards

## When to Use

- Writing new `.feature` files or step definitions
- Reviewing existing Playwright BDD tests for compliance
- Refactoring tests that duplicate steps, hardcode selectors, or break architecture rules
- Adding a new page object or flow method
- Migrating existing tests to follow new standards
- Deciding where logic belongs (feature vs step vs flow vs page object)

---

## Architecture

All tests follow a strict layered architecture:

```
Feature File → Step Definitions → Flow Layer → Page Objects → BasePage
                                                          ↑
                                                     Test Data
```

| Layer | File(s) | Responsibility |
|-------|---------|----------------|
| Feature file | `*.feature` | Business-readable scenarios in Given/When/Then |
| Step definitions | `*.steps.ts` | Thin — call flows or page methods only |
| Common steps | `common.steps.ts` | Shared reusable steps (e.g. `I click Continue`) |
| Flow layer | `flows/*.ts` | Journey orchestration using business-named methods |
| Page objects | `pages/*.page.ts` | Locators, UI actions, UI assertions |
| BasePage | `base.page.ts` | Shared behaviour inherited by all page objects |
| Test data | `data/*.ts` | Centralised, reusable fixtures |

---

## 1. Feature File Rules

### Required scenarios for every page

Every page feature file **must** include:

1. **Content validation** — verify page heading, back link, Continue button, inputs
2. **Happy path (E2E)** — navigate, interact, assert next page
3. **Negative scenarios** — missing/invalid input triggers error summary

### Structure

```gherkin
@<page>
Feature: <Page Name>

  @<page> @content-validation @smoke @regression
  Scenario Outline: <Page Name> page content validation
    Given I navigate to the "<Page Name>" page for "<target>"
    Then I should be on the "<Page Name>" page
    And the page heading should be "..."
    And the back link should be visible
    And the Continue button should be visible

  @<page> @e2e @regression
  Scenario Outline: Candidate can continue after entering valid details
    Given I navigate to the "<Page Name>" page for "<target>"
    When I enter valid ... for "<target>"
    And I click Continue
    Then I should be on the "<Next Page>" page

  @<page> @negative @regression
  Scenario Outline: Candidate cannot continue when required details are missing
    Given I navigate to the "<Page Name>" page for "<target>"
    When I click Continue
    Then I should see the error summary

    Examples:
      | target |
      | GB     |
      | NI     |
```

### Rules

- Use `Scenario Outline` + `Examples` table for region variations (GB/NI) — never duplicate scenarios
- Scenarios must be small and focused — one behaviour per scenario
- Use business language only — no selectors, DOM, locator, URL paths, or CSS class names
- Step wording must be reusable across pages (not page-specific)

---

## 2. Tagging Standard

Tags must follow the pattern: `@<page> @<purpose> @<type>`

| Category | Examples |
|----------|---------|
| Page | `@candidate-details`, `@enter-email` |
| Purpose | `@content-validation`, `@e2e`, `@negative` |
| Test type | `@smoke`, `@regression` |
| Utility | `@cleanup`, `@flaky` |

**Example:** `@candidate-details @content-validation @smoke @regression`

Tags control execution via npm scripts:
```json
{
  "test:smoke": "playwright test --grep @smoke",
  "test:regression": "playwright test --grep @regression"
}
```

---

## 3. Step Definitions Rules

Steps must be **thin**. The four rules:

1. **No locators in steps** — delegate to page objects
2. **No assertion logic in steps** — delegate to page object `verify*` methods
3. **Steps call page or flow methods only**
4. **Step wording must be reusable across all pages**

```ts
// GOOD
Then('the Continue button should be visible', async ({ pages }) => {
  const page = await resolveCurrentPage(pages);
  await page.verifyContinueButtonVisible();
});

// BAD
Then('the Continue button should be visible', async ({ page }) => {
  await expect(page.locator('[data-id="continue"]')).toBeVisible();
});
```

```ts
// GOOD — generic, reusable
When('I click Continue', async ({ pages }) => {
  const page = await resolveCurrentPage(pages);
  await page.clickContinue();
});

// BAD — page-specific step name
When('I click Continue on Candidate Details', async ({ candidateDetailsPage }) => {
  await candidateDetailsPage.clickContinue();
});
```

---

## 4. Common Steps

Shared steps live in `common.steps.ts`. Do **not** duplicate them in page step files.

Common steps include:
- `When I click Continue`
- `Then I should see the error summary`
- `Then the Continue button should be visible`
- `Then the back link should be visible`
- `Then I should be on the {string} page`

---

## 5. Dynamic Page Resolution

Use `resolveCurrentPage(pages)` in common steps — never hardcode the page name.

```ts
async function resolveCurrentPage(pages: any) {
  const pageObjects = Object.values(pages);
  for (const page of pageObjects) {
    try {
      if (typeof page.pageHeading === 'function') {
        if (await page.pageHeading().isVisible()) {
          return page;
        }
      }
    } catch {}
  }
  throw new Error('No page matched for current context');
}
```

This automatically supports new pages and keeps steps generic.

---

## 6. Flow Layer

Flows represent user journeys. Use business-named methods; call page objects internally.

```ts
// GOOD
await flows.booking.start(target);
await flows.booking.chooseNoSupport();
await flows.booking.enterValidCandidateDetails(target);

// BAD
await page.locator('#start-booking').click();
await page.locator('#firstName').fill('John');
```

Flow rules:
- No raw locators in flows
- Use business naming (`enterValidCandidateDetails`, not `fillForm`)
- Call page objects internally

---

## 7. Page Objects

Responsibilities:
- Define locators (only here — nowhere else)
- Perform UI actions
- Handle UI assertions via `verify*` methods

```ts
// GOOD
async verifyInputFields(): Promise<void> {
  await expect(this.firstName()).toBeVisible();
  await expect(this.surname()).toBeVisible();
}

// BAD — locators leaking into step definitions
Then('I should see candidate inputs', async ({ page }) => {
  await expect(page.locator('#firstName')).toBeVisible();
});
```

---

## 8. BasePage (Mandatory)

All page objects extend `BasePage`. Shared behaviour lives here — never duplicated.

```ts
export class BasePage {
  constructor(protected readonly page: Page) {}

  continueButton() {
    return this.page.getByRole('button', { name: 'Continue' });
  }
  async clickContinue(): Promise<void> {
    await this.continueButton().click();
  }
  async verifyContinueButtonVisible(): Promise<void> {
    await expect(this.continueButton()).toBeVisible();
  }
  pageHeading() {
    return this.page.getByRole('heading', { level: 1 });
  }
}

export class CandidateDetailsPage extends BasePage {
  // page-specific methods only
}
```

Shared methods belong in `BasePage`:
- `verifyContinueButtonVisible()`
- `clickContinue()`
- `verifyErrorSummary()`
- `pageHeading()`

---

## 9. Test Data

Test data is centralised in `data/*.ts` — never hardcoded inline.

```ts
// GOOD
const candidate = e2eCandidates.e2eBookATheoryTest.gbCandidate;
await candidateDetailsPage.enterDetails(candidate);

// BAD
await page.locator('#firstName').fill('John');
await page.locator('#drivingLicenceNumber').fill('SMITH901010J99AA');
```

Structure test data exports by journey and region:
```ts
export const e2eCandidates = {
  e2eBookATheoryTest: {
    gbCandidate: { firstName: 'John', surname: 'Smith', ... },
    niCandidate: { ... },
  },
};
```

---

## 10. Execution Rules

- Tests must be **independent** — no shared state between scenarios
- Tests must be **parallel-safe** — no `test.describe.configure({ mode: 'serial' })`
- Use tags to control execution scope, not file order

```ts
// BAD — serial dependency
let candidateReference: string;
test('creates candidate', async ({ page }) => {
  candidateReference = await createCandidate(page);
});
test('uses candidate', async ({ page }) => {
  await searchForCandidate(page, candidateReference); // fails if run alone
});
```

---

## 11. Pre-commit Checklist (MANDATORY)

Before committing any test:

- [ ] Scenario reads like business behaviour, not technical steps
- [ ] Given / When / Then used correctly
- [ ] No locators, selectors, or DOM terms in feature files or step definitions
- [ ] Flows used for journey orchestration
- [ ] Page objects contain all UI logic
- [ ] Tags follow `@<page> @<purpose> @<type>` pattern
- [ ] No duplicate step wording — shared steps live in `common.steps.ts`
- [ ] `BasePage` methods used for shared behaviour
- [ ] Test data is centralised
- [ ] Tests are independent and parallel-safe

---

## Quick Reference Cheat Sheet

| Area | Do | Avoid |
|------|----|-------|
| Feature files | Business language in Given/When/Then | Selectors, DOM, URLs, locators |
| Steps | Thin — call page or flow methods | Assertions, locators, logic in steps |
| Common steps | Put reusable steps in `common.steps.ts` | Duplicating steps across page files |
| Flows | Orchestrate journeys with business methods | Raw Playwright actions in flows |
| Page objects | Locators, actions, assertions here only | Exposing selectors outside page layer |
| BasePage | Shared behaviour: Continue, Back, error summary | Duplicating common methods per page |
| Test data | Centralised fixtures | Hardcoded names/dates/licence numbers |
| Tags | `@page @purpose @type` together | Vague or inconsistent tag names |
| Execution | Independent, parallel-safe, tag-controlled | Serial mode, shared state between tests |

**Golden pattern:** `Feature → Steps → Flow → Page Object → BasePage → Test Data`
