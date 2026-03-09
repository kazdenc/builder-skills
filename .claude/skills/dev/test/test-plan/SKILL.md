---
name: test-plan
description: Write a test strategy covering what to test, coverage targets, and testing pyramid distribution. Use when user says "test plan", "testing strategy", "what should I test", "test coverage", "how to test this", or needs to plan testing for a feature or project before writing tests.
user-invokable: true
args:
  - name: target
    description: The feature or project to plan tests for (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

# Test Plan

Write a test strategy that answers two questions clearly: **what deserves testing** and **how much testing is enough**. Everything else is execution detail.

## Step 1: Understand What's Being Tested

Before planning any tests, nail down what you're working with. Read the code or requirements. If the user hasn't provided enough context, ask.

| Input | Question | Watch out for |
|-------|----------|---------------|
| **Code / Feature** | What does this do? What are the inputs and outputs? | Planning tests for code you haven't read |
| **Critical paths** | Which flows would break the product if they failed? | Treating all code paths as equally important |
| **Dependencies** | What external systems, APIs, or services does it touch? | Ignoring side effects and integrations |
| **Edge cases** | What unusual inputs or states could occur? | Only testing the happy path |

If existing tests, specs, or prior conversations exist, pull from them. Don't invent context.

## Step 2: Apply the Testing Pyramid

Distribute test effort deliberately. The pyramid exists because fast feedback loops matter more than exhaustive simulation.

| Layer | Share | What to test | Characteristics |
|-------|-------|-------------|-----------------|
| **Unit tests** | ~70% | Pure functions, utilities, hooks, reducers, transformers | Fast, isolated, many. Test one thing per test. Mock nothing or mock only external I/O. |
| **Integration tests** | ~20% | Component interactions, API routes, database queries, service boundaries | Test contracts between units. Verify that pieces work together correctly. Slower, fewer. |
| **E2E tests** | ~10% | Critical user flows only: signup, checkout, core CRUD | Slow, flaky-prone, expensive. Reserve for flows where failure = revenue loss or user churn. |

These ratios are targets, not rules. A utility library might be 95% unit tests. A UI-heavy app might lean harder on integration tests. Adjust with intent.

## Step 3: Decide What to Test

Map each piece of functionality to the right test type. Use this decision table.

| What you're testing | Test type | Why |
|---------------------|-----------|-----|
| Business logic, calculations, data transforms | Unit test | Fast feedback, easy to cover all branches |
| User-facing flows (form submit, navigation) | Integration or E2E | Need to verify real DOM and event behavior |
| Edge cases (null inputs, empty arrays, boundaries) | Unit test | Cheap to write, high defect-prevention value |
| Visual appearance and layout | Snapshot test (use sparingly) | Brittle; prefer visual regression tools for critical UI |
| API request/response contracts | Integration test | Verify serialization, status codes, error shapes |
| Error handling and failure modes | Unit + integration | Unit for thrown errors, integration for error boundaries and fallback UI |
| State management (stores, reducers) | Unit test | Pure logic, deterministic, fast to test |
| Third-party integrations | Integration test with mocks | Verify your code handles their API correctly |

## Step 4: Define Coverage Targets

Aim for meaningful coverage, not 100%. Chasing full coverage leads to brittle tests that test implementation details.

| Coverage target | When it makes sense |
|-----------------|---------------------|
| **90%+** | Shared libraries, payment logic, auth flows — code where bugs have outsized impact |
| **70-80%** | Application code, feature modules — good balance of safety and velocity |
| **50-70%** | Rapidly prototyping, exploratory features — cover critical paths, skip the rest |
| **Skip coverage metrics** | One-off scripts, throwaway prototypes, generated code |

Focus coverage on:
- Code with high cyclomatic complexity (many branches)
- Code that handles money, auth, or user data
- Code that's changed frequently (high churn = high risk)

## Step 5: Choose Tools

Pick tools that match your stack. Don't over-engineer the test setup.

| Purpose | Tool | Notes |
|---------|------|-------|
| Unit tests | **Vitest** | Fast, ESM-native, Jest-compatible API. Preferred for modern projects. |
| Component tests | **Testing Library** (@testing-library/react) | Test behavior, not implementation. Queries by role, text, label. |
| User interaction | **@testing-library/user-event** | Simulates real user events (click, type, tab). Prefer over fireEvent. |
| API mocking | **msw** (Mock Service Worker) | Intercepts at the network level. Works in tests and browser dev. |
| E2E tests | **Playwright** | Cross-browser, reliable, good DX. Prefer over Cypress for new projects. |
| Visual regression | **Playwright screenshots** or **Chromatic** | Use only for design-critical UI. Not a substitute for functional tests. |

## Output Format

Deliver the test plan as a prioritized list of test cases grouped by type.

```
# Test Plan: [Feature / Component Name]

## Unit Tests (priority order)
- [ ] [function/module] — [what behavior to verify]
- [ ] [function/module] — [edge case to cover]

## Integration Tests (priority order)
- [ ] [interaction/flow] — [what contract to verify]
- [ ] [API route] — [request/response shape to verify]

## E2E Tests (priority order)
- [ ] [critical user flow] — [what end-to-end behavior to verify]

## Coverage Notes
- Target: [X%] for [reason]
- Skip: [what's deliberately not tested and why]

## Open Questions
- [Anything unresolved about test approach]
```

## Anti-Patterns to Avoid

| Anti-pattern | Problem | Instead |
|-------------|---------|---------|
| Testing implementation details | Tests break on every refactor, provide no confidence | Test inputs/outputs and observable behavior |
| Mocking everything | Tests pass but nothing actually works together | Mock only external I/O; let units collaborate |
| Snapshot overuse | Giant snapshots nobody reviews, approved blindly | Use snapshots only for small, stable structures |
| Testing the framework | Verifying that React renders or that Vitest asserts | Test your logic, not the tool's behavior |
| 100% coverage as a goal | Diminishing returns, brittle tests, wasted time | Cover critical paths thoroughly, accept gaps in trivial code |
| Copy-paste test cases | Hard to maintain, masks missing abstractions | Use test.each() or parameterized tests for repetitive cases |
