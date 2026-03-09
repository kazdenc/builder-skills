---
name: test-write
description: Generate tests for existing code including unit, integration, and e2e tests. Uses Vitest and Testing Library by default. Use when user says "write tests", "add tests", "test this function", "test this component", "need test coverage", or wants tests written for existing code.
user-invokable: true
args:
  - name: target
    description: The file, function, or component to test (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

# Write Tests

Generate tests that catch real bugs and survive refactors. Every test should answer: **what behavior breaks if this test fails?**

## Step 1: Read the Code

Before writing any test, understand the code under test. If the user provides a file or function, read it first.

| What to identify | Why it matters | Watch out for |
|-----------------|----------------|---------------|
| **Inputs** | These become your test parameters | Assuming only happy-path inputs |
| **Outputs / return values** | These are your assertions | Testing internal state instead of outputs |
| **Side effects** | These need mocking or verification | Missing async side effects, event emissions |
| **Edge cases** | These are where bugs hide | null, undefined, empty arrays, boundary values, concurrent calls |
| **Error paths** | These need explicit test coverage | Only testing success cases |

## Step 2: Determine Test Type

Match the code to the right kind of test.

| Code under test | Test type | Tools |
|----------------|-----------|-------|
| Pure function (util, helper, transformer) | Unit test | Vitest |
| React component | Component test | Vitest + @testing-library/react + @testing-library/user-event |
| React hook | Hook test | Vitest + @testing-library/react (renderHook) |
| API route / server handler | Integration test | Vitest + supertest or direct handler invocation |
| Full user flow (multi-page, auth) | E2E test | Playwright |
| Redux / Zustand store | Unit test | Vitest |

## Step 3: Write Tests Using the AAA Pattern

Structure every test as **Arrange, Act, Assert**. This keeps tests readable and intention-revealing.

```typescript
describe('functionName', () => {
  it('should [expected behavior] when [condition]', () => {
    // Arrange — set up inputs and dependencies
    const input = { name: 'test', value: 42 };

    // Act — call the code under test
    const result = functionName(input);

    // Assert — verify the output
    expect(result).toEqual({ processed: true, name: 'test' });
  });
});
```

## Test Naming Convention

Use `describe` for the unit, `it` for the behavior. Name tests so a failure message reads like a sentence.

```
describe('calculateTotal')
  it('should return 0 when cart is empty')
  it('should sum item prices including quantity')
  it('should apply percentage discount to subtotal')
  it('should throw when discount exceeds 100%')
```

## Common Test Patterns

### Testing Async Functions

```typescript
it('should resolve with user data when API call succeeds', async () => {
  const user = await fetchUser('user-123');
  expect(user).toEqual({ id: 'user-123', name: 'Test' });
});

it('should reject when user is not found', async () => {
  await expect(fetchUser('nonexistent')).rejects.toThrow('User not found');
});
```

### Testing React Hooks

```typescript
import { renderHook, act } from '@testing-library/react';

it('should increment counter', () => {
  const { result } = renderHook(() => useCounter(0));

  act(() => {
    result.current.increment();
  });

  expect(result.current.count).toBe(1);
});
```

### Testing React Components

```typescript
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';

it('should submit form with entered values', async () => {
  const onSubmit = vi.fn();
  const user = userEvent.setup();
  render(<LoginForm onSubmit={onSubmit} />);

  await user.type(screen.getByLabelText('Email'), 'test@example.com');
  await user.type(screen.getByLabelText('Password'), 'password123');
  await user.click(screen.getByRole('button', { name: 'Log in' }));

  expect(onSubmit).toHaveBeenCalledWith({
    email: 'test@example.com',
    password: 'password123',
  });
});
```

### Testing Components with Async State

```typescript
import { render, screen, waitFor } from '@testing-library/react';

it('should display user name after loading', async () => {
  render(<UserProfile userId="123" />);

  expect(screen.getByText('Loading...')).toBeInTheDocument();

  await waitFor(() => {
    expect(screen.getByText('Jane Doe')).toBeInTheDocument();
  });
});
```

### Testing API Routes (with mocking)

```typescript
import { http, HttpResponse } from 'msw';
import { setupServer } from 'msw/node';

const server = setupServer(
  http.get('/api/users/:id', ({ params }) => {
    return HttpResponse.json({ id: params.id, name: 'Test User' });
  })
);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

it('should handle API error gracefully', async () => {
  server.use(
    http.get('/api/users/:id', () => {
      return new HttpResponse(null, { status: 500 });
    })
  );

  render(<UserProfile userId="123" />);

  await waitFor(() => {
    expect(screen.getByText('Failed to load user')).toBeInTheDocument();
  });
});
```

### Testing Error Cases

```typescript
it('should throw TypeError when input is null', () => {
  expect(() => processData(null)).toThrow(TypeError);
});

it('should render error boundary fallback on component error', () => {
  const BrokenComponent = () => { throw new Error('boom'); };

  render(
    <ErrorBoundary fallback={<p>Something went wrong</p>}>
      <BrokenComponent />
    </ErrorBoundary>
  );

  expect(screen.getByText('Something went wrong')).toBeInTheDocument();
});
```

## Edge Cases to Always Consider

Include these in every test suite. They catch the bugs that slip past happy-path testing.

| Edge case | Example test |
|-----------|-------------|
| **null / undefined inputs** | `processData(null)` — should throw or return default |
| **Empty arrays / objects** | `calculateTotal([])` — should return 0, not NaN |
| **Boundary values** | `paginate(items, { page: 0 })` — zero, negative, max int |
| **Concurrent calls** | Two rapid clicks on submit — should not double-submit |
| **Large inputs** | 10,000-item array — should not crash or hang |
| **Unicode / special characters** | `search('caf\u00e9')` — should handle diacritics |
| **Type coercion traps** | `compare('1', 1)` — should use strict equality |

## Code Conventions

| Convention | Rationale |
|-----------|-----------|
| One assertion per test (when practical) | Pinpoints exactly what failed. Use multiple asserts only when they verify one logical behavior. |
| No test interdependency | Every test must pass in isolation. Never rely on execution order or shared mutable state. |
| Use factories, not fixtures | `createUser({ name: 'Test' })` is flexible. `fixtures/user.json` is rigid and hides intent. |
| Colocate tests with source | `utils.ts` and `utils.test.ts` in the same directory. Easy to find, easy to maintain. |
| Use `vi.fn()` for spies | Track calls and arguments. Prefer over manual tracking variables. |
| Clean up after tests | Use `afterEach` for DOM cleanup, timer restoration, server reset. Leaking state causes flaky tests. |
| Avoid `test.skip` accumulation | Skipped tests rot. Fix or delete within one sprint. |

## Default Tools

| Tool | Purpose | Install |
|------|---------|---------|
| **Vitest** | Test runner and assertion library | `npm i -D vitest` |
| **@testing-library/react** | Component rendering and queries | `npm i -D @testing-library/react` |
| **@testing-library/user-event** | Realistic user interaction simulation | `npm i -D @testing-library/user-event` |
| **@testing-library/jest-dom** | Extended DOM matchers (toBeInTheDocument, etc.) | `npm i -D @testing-library/jest-dom` |
| **msw** | Network-level API mocking | `npm i -D msw` |
| **Playwright** | E2E browser testing | `npm i -D @playwright/test` |

## When Tests Are Done

Deliver tests that meet these checks:

- **Every test has a clear name.** Reading the test name alone tells you what broke.
- **No implementation details tested.** Tests don't reference internal variable names, CSS classes (prefer roles), or private methods.
- **Edge cases are covered.** At minimum: null input, empty collection, error path.
- **Tests run independently.** Shuffle the order — they should still pass.
- **Mocks are minimal.** Only mock what crosses a boundary (network, file system, time). Let everything else run for real.
