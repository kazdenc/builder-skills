---
name: typescript-patterns
description: TypeScript best practices and advanced type design patterns. Use when writing, reviewing, or refactoring TypeScript code. Covers type narrowing, generics, utility types, discriminated unions, error handling, and module patterns. Triggers on TypeScript, type safety, or type design tasks.
metadata:
  author: builder-skills
  version: "1.0.0"
---

# TypeScript Patterns

Apply these patterns when writing, reviewing, or refactoring TypeScript code. Prefer type safety over convenience. Catch errors at compile time, not runtime.

## Type Design Principles

Default to the narrowest type that accurately represents the data. Widen only when you have a concrete reason.

| Principle | Do | Don't |
|-----------|-----|-------|
| Narrow types | `status: "active" \| "inactive"` | `status: string` |
| Branded types for IDs | `type UserId = string & { __brand: "UserId" }` | `userId: string` (mixable with any string) |
| Discriminated unions | `{ kind: "circle"; radius: number } \| { kind: "rect"; w: number; h: number }` | Type assertions to distinguish shapes |
| Readonly by default | `readonly items: Item[]` | Mutable arrays unless mutation is required |
| Explicit return types on public APIs | `function getUser(id: UserId): Promise<User>` | Inferred return types on exported functions |

### Branded Types

Use branded types to prevent accidental mixing of structurally identical values:

```typescript
type UserId = string & { readonly __brand: unique symbol };
type OrderId = string & { readonly __brand: unique symbol };

function createUserId(id: string): UserId {
  return id as UserId;
}

function getOrder(orderId: OrderId): Order { /* ... */ }

// Compile error: UserId is not assignable to OrderId
getOrder(createUserId("abc"));
```

## Generics

Use generics when a function or type operates on a value whose type the caller determines. Don't add generics speculatively — add them when you have two or more concrete use cases.

### Constraining Generics

Always constrain generics to the narrowest interface they need:

```typescript
// Good — constrained to what the function actually uses
function getProperty<T extends Record<string, unknown>, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

// Bad — unconstrained, anything goes
function getProperty<T>(obj: T, key: string): unknown { /* ... */ }
```

### Common Generic Patterns

**Factory pattern** — return typed instances:

```typescript
function createRepository<T extends { id: string }>(collection: string) {
  return {
    findById(id: string): Promise<T | null> { /* ... */ },
    save(entity: T): Promise<void> { /* ... */ },
    delete(id: string): Promise<void> { /* ... */ },
  };
}

const users = createRepository<User>("users");
```

**Builder pattern** — chain methods with progressive type narrowing:

```typescript
class QueryBuilder<T> {
  where<K extends keyof T>(field: K, value: T[K]): this { /* ... */ }
  select<K extends keyof T>(...fields: K[]): QueryBuilder<Pick<T, K>> { /* ... */ }
  build(): Query<T> { /* ... */ }
}
```

**Inference helper** — let TypeScript infer from arguments:

```typescript
function createAction<TInput, TOutput>(config: {
  input: z.ZodSchema<TInput>;
  handler: (input: TInput) => Promise<TOutput>;
}) { /* ... */ }

// TInput and TOutput are inferred from usage
createAction({
  input: z.object({ name: z.string() }),
  handler: async (input) => ({ id: "1", name: input.name }),
});
```

## Utility Types

Use built-in utility types instead of hand-rolling equivalents. Here is when to reach for each:

| Utility Type | Use When | Example |
|-------------|----------|---------|
| `Pick<T, K>` | You need a subset of properties | `Pick<User, "id" \| "name">` for a summary view |
| `Omit<T, K>` | You need everything except certain properties | `Omit<User, "password">` for a public response |
| `Partial<T>` | All properties become optional (patch/update payloads) | `Partial<User>` for an update endpoint |
| `Required<T>` | All properties become required | `Required<Config>` after merging with defaults |
| `Record<K, V>` | You need a typed dictionary | `Record<StatusCode, string>` for a lookup table |
| `Extract<T, U>` | Pull members of a union that match a condition | `Extract<Event, { kind: "click" }>` |
| `Exclude<T, U>` | Remove members of a union that match a condition | `Exclude<Status, "deleted">` |
| `NonNullable<T>` | Strip `null` and `undefined` from a type | `NonNullable<string \| null>` after a null check |
| `ReturnType<T>` | Get the return type of a function | `ReturnType<typeof fetchUser>` to type a variable |
| `Parameters<T>` | Get the parameter types of a function as a tuple | `Parameters<typeof handler>[0]` for the first arg |

Combine them for precision: `Partial<Pick<User, "name" | "email">>` for an optional name-and-email update.

## Error Handling

Never throw from library or shared code. Use the Result pattern to make errors part of the type signature.

### Result Type

```typescript
type Result<T, E = Error> =
  | { ok: true; data: T }
  | { ok: false; error: E };
```

Apply it consistently:

```typescript
type ValidationError = { field: string; message: string };
type AuthError = { code: "UNAUTHORIZED" | "FORBIDDEN"; message: string };

async function createUser(input: CreateUserInput): Promise<Result<User, ValidationError[]>> {
  const errors = validate(input);
  if (errors.length > 0) {
    return { ok: false, error: errors };
  }
  const user = await db.users.create(input);
  return { ok: true, data: user };
}

// Caller handles both paths explicitly
const result = await createUser(input);
if (!result.ok) {
  // result.error is typed as ValidationError[]
  return showErrors(result.error);
}
// result.data is typed as User
console.log(result.data.id);
```

### Typed Error Hierarchies

Use discriminated unions for error types so callers can narrow:

```typescript
type AppError =
  | { kind: "validation"; fields: { field: string; message: string }[] }
  | { kind: "not_found"; resource: string; id: string }
  | { kind: "unauthorized"; reason: string }
  | { kind: "internal"; message: string };
```

### When to Throw

Throw only at application boundaries (request handlers, CLI entry points) where you convert Results into HTTP responses or exit codes. Never throw from utility functions, services, or domain logic.

## Discriminated Unions

Use discriminated unions for any value that can be in one of several mutually exclusive states. Always use a `kind`, `type`, or `status` field as the discriminant.

### State Machines

```typescript
type RequestState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; error: AppError };
```

### API Responses

```typescript
type ApiResponse<T> =
  | { ok: true; data: T; meta: { requestId: string } }
  | { ok: false; error: { code: string; message: string; details?: unknown } };
```

### Form States

```typescript
type FormState =
  | { step: "input"; values: Partial<FormValues> }
  | { step: "review"; values: FormValues }
  | { step: "submitting"; values: FormValues }
  | { step: "complete"; result: SubmitResult }
  | { step: "error"; values: FormValues; error: string };
```

### Exhaustiveness Checking

Always use a `never` check in switch statements to catch unhandled variants at compile time:

```typescript
function assertNever(value: never): never {
  throw new Error(`Unhandled variant: ${JSON.stringify(value)}`);
}

function renderState(state: RequestState<User>) {
  switch (state.status) {
    case "idle": return null;
    case "loading": return <Spinner />;
    case "success": return <Profile user={state.data} />;
    case "error": return <ErrorBanner error={state.error} />;
    default: return assertNever(state);
  }
}
```

## Module Patterns

### Barrel Exports

Use barrel exports (`index.ts`) sparingly. They hurt tree-shaking and create circular dependency risks. Prefer direct imports.

| Scenario | Use Barrel? | Why |
|----------|------------|-----|
| Public API of a package/library | Yes | Provides a stable import surface |
| Internal module grouping | No | Import directly from the source file |
| Re-exporting types only | Yes | Types are erased at compile time, no bundle cost |

### Re-export Types

When a module's types are needed elsewhere but its runtime code is not, re-export types separately:

```typescript
// types.ts — pure types, no runtime
export type { User, CreateUserInput, UserRole };

// index.ts — runtime exports
export { createUser, getUser, deleteUser };
```

### Isolate Side Effects

Keep side effects (database connections, env reads, logging init) in dedicated files. Don't mix side-effect code with pure business logic:

```typescript
// db.ts — side effect: opens connection
export const db = createConnection(process.env.DATABASE_URL);

// user-service.ts — pure, receives db as dependency
export function createUserService(db: Database) {
  return {
    findById(id: UserId): Promise<User | null> { /* ... */ },
  };
}
```

## Anti-Patterns

Avoid these. When you encounter them in existing code, refactor.

| Anti-Pattern | Problem | Do Instead |
|-------------|---------|-----------|
| `any` | Disables all type checking | Use `unknown` and narrow, or define the actual type |
| `as` type assertions | Lies to the compiler | Use type guards or discriminated unions to narrow |
| Non-null assertion `!` | Hides potential null bugs | Check for null explicitly or use optional chaining |
| `enum` | Non-standard JS, poor tree-shaking | Use `as const` objects or string literal unions |
| `Function` type | Accepts anything callable | Use specific signatures: `(input: T) => R` |
| `Object` / `{}` | Matches almost everything | Use `Record<string, unknown>` or a specific interface |
| `namespace` | Legacy pattern, poor module compat | Use ES modules |
| Overloads for unions | Verbose, hard to maintain | Use a single signature with a discriminated union parameter |
| `@ts-ignore` | Silences real errors | Fix the type error or use `@ts-expect-error` with a comment |
| Index signatures everywhere | Too permissive | Define explicit interfaces for known shapes |
