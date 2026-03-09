---
name: api-design
description: RESTful API design conventions covering URL structure, HTTP methods, error formats, pagination, versioning, and authentication patterns. Use when designing, reviewing, or implementing APIs. Triggers on API design, endpoint structure, REST conventions, or backend architecture tasks.
metadata:
  author: builder-skills
  version: "1.0.0"
---

# API Design

Apply these conventions when designing, reviewing, or implementing RESTful APIs. Consistency matters more than cleverness — follow the patterns below.

## URL Structure

Use nouns for resources, not verbs. Resources are things, not actions.

| Rule | Correct | Incorrect |
|------|---------|-----------|
| Plural nouns | `/users`, `/orders` | `/user`, `/getOrders` |
| Nouns, not verbs | `POST /users` | `POST /createUser` |
| Kebab-case for multi-word | `/line-items` | `/lineItems`, `/line_items` |
| Nest for relationships (max 2 levels) | `/users/:id/orders` | `/users/:id/orders/:orderId/items/:itemId` |
| Flat when parent is obvious | `/orders/:orderId/items` | `/users/:userId/orders/:orderId/items` |

### Query Parameters

Use query parameters for filtering, sorting, searching, and pagination. Never encode these in the URL path.

```
GET /users?status=active&sort=-created_at&limit=20&cursor=abc123
GET /orders?user_id=42&min_total=100&fields=id,status,total
```

| Parameter | Convention | Example |
|-----------|-----------|---------|
| Filter | Field name as key | `?status=active&role=admin` |
| Sort | Field name, prefix `-` for descending | `?sort=-created_at,name` |
| Search | `q` parameter | `?q=john` |
| Field selection | `fields` comma-separated | `?fields=id,name,email` |
| Pagination | `cursor` or `offset`/`limit` | `?cursor=abc&limit=20` |

## HTTP Methods

| Method | Purpose | Idempotent | Request Body | Success Code | Response Body |
|--------|---------|-----------|-------------|-------------|---------------|
| `GET` | Retrieve resource(s) | Yes | No | `200` | Resource or collection |
| `POST` | Create a new resource | No | Yes | `201` | Created resource |
| `PUT` | Replace a resource entirely | Yes | Yes | `200` | Updated resource |
| `PATCH` | Partially update a resource | No* | Yes | `200` | Updated resource |
| `DELETE` | Remove a resource | Yes | No | `204` | No body |

*PATCH is not inherently idempotent but can be implemented as such. Treat it as non-idempotent by default.

**Key rules:**
- GET must never mutate state. Ever.
- PUT replaces the entire resource — omitted fields are removed or reset to defaults.
- PATCH updates only the fields included in the body.
- POST to a collection creates a new resource. Return the created resource with its `id`.
- DELETE should succeed silently if the resource is already gone (idempotent).

## Status Codes

Use the most specific appropriate code. Don't return 200 for everything.

### 2xx — Success

| Code | Meaning | Use When |
|------|---------|----------|
| `200 OK` | Request succeeded | GET, PUT, PATCH success |
| `201 Created` | Resource created | POST success — include `Location` header |
| `204 No Content` | Success, no body | DELETE success, or PUT/PATCH when no body is needed |

### 4xx — Client Error

| Code | Meaning | Use When |
|------|---------|----------|
| `400 Bad Request` | Malformed request | Invalid JSON, missing required fields, validation failures |
| `401 Unauthorized` | Not authenticated | Missing or invalid authentication token |
| `403 Forbidden` | Authenticated but not allowed | User lacks permission for this action |
| `404 Not Found` | Resource doesn't exist | ID not found, or route doesn't exist |
| `405 Method Not Allowed` | Wrong HTTP method | POST to a read-only resource |
| `409 Conflict` | State conflict | Duplicate creation, version mismatch |
| `422 Unprocessable Entity` | Semantic validation failure | Valid JSON, but business rules violated |
| `429 Too Many Requests` | Rate limit exceeded | Include `Retry-After` header |

### 5xx — Server Error

| Code | Meaning | Use When |
|------|---------|----------|
| `500 Internal Server Error` | Unexpected failure | Unhandled exception — log it, don't leak details |
| `502 Bad Gateway` | Upstream failure | A dependency returned an invalid response |
| `503 Service Unavailable` | Temporarily down | Maintenance or overload — include `Retry-After` |

## Error Format

Return errors in a consistent shape. Every error response uses this structure:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Request body failed validation.",
    "details": [
      { "field": "email", "message": "Must be a valid email address." },
      { "field": "age", "message": "Must be at least 18." }
    ]
  }
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `error.code` | `string` | Yes | Machine-readable error code (UPPER_SNAKE_CASE) |
| `error.message` | `string` | Yes | Human-readable summary |
| `error.details` | `array \| object` | No | Field-level errors, additional context |

**Rules:**
- Always return the same top-level `{ error: { ... } }` shape for all error responses.
- Use `code` for programmatic handling (clients switch on it), `message` for display.
- Never expose stack traces, internal paths, or database errors in production.
- Include a `request_id` in error responses (or top-level meta) for debugging.

## Pagination

### Cursor-Based (Preferred)

Use cursor-based pagination by default. It handles inserts/deletes gracefully and scales to large datasets.

**Request:**
```
GET /users?limit=20&cursor=eyJpZCI6MTAwfQ
```

**Response:**
```json
{
  "data": [ { "id": 101, "name": "Ada" }, { "id": 102, "name": "Bob" } ],
  "pagination": {
    "next_cursor": "eyJpZCI6MTAyfQ",
    "has_more": true
  }
}
```

| Field | Description |
|-------|-------------|
| `next_cursor` | Opaque string — encode whatever your DB needs (usually last ID) |
| `has_more` | Boolean — tells client whether to fetch again |

### Offset-Based (When Needed)

Use offset pagination only when users need to jump to arbitrary pages (admin tables, search results).

**Request:**
```
GET /users?offset=40&limit=20
```

**Response:**
```json
{
  "data": [ { "id": 41, "name": "Ada" } ],
  "pagination": {
    "offset": 40,
    "limit": 20,
    "total": 523
  }
}
```

**Warning:** Offset pagination degrades on large datasets (DB must skip `offset` rows). Never use for infinite scroll.

## Authentication

### Bearer Tokens (Preferred for User Auth)

Pass tokens in the `Authorization` header:

```
Authorization: Bearer eyJhbGciOiJSUzI1NiIs...
```

- Use short-lived access tokens (15 min–1 hr) with refresh tokens.
- Validate tokens on every request. Don't cache validation results.
- Return `401` for missing/expired tokens, `403` for insufficient permissions.

### API Keys (For Server-to-Server)

Pass API keys in a custom header or as a query parameter (header preferred):

```
X-API-Key: sk_live_abc123def456
```

| Aspect | Recommendation |
|--------|---------------|
| Where to send | `X-API-Key` header (preferred) or `Authorization: ApiKey <key>` |
| Key format | Prefix with environment: `sk_live_`, `sk_test_` |
| Storage | Hash keys server-side, never store plaintext |
| Rotation | Support multiple active keys for zero-downtime rotation |

### When to Use Each

| Scenario | Auth Method |
|----------|------------|
| Browser/mobile app user sessions | Bearer token (OAuth 2.0 / JWT) |
| Server-to-server integration | API key |
| Third-party developer access | OAuth 2.0 with scopes |
| Internal microservices | mTLS or signed JWTs |

## Versioning

### URL Versioning (Preferred)

Prefix major versions in the URL path:

```
GET /v1/users
GET /v2/users
```

- Version only when you introduce breaking changes.
- Run old versions in parallel during migration. Set a sunset date and communicate it.
- Non-breaking additions (new optional fields, new endpoints) do not require a version bump.

### Header Versioning (Alternative)

```
Accept: application/vnd.myapi.v2+json
```

Use header versioning when URL versioning creates routing complexity or you need fine-grained version control. URL versioning is simpler for most cases.

### What Counts as a Breaking Change

| Breaking | Not Breaking |
|----------|-------------|
| Removing a field from a response | Adding a new optional field to a response |
| Renaming a field | Adding a new endpoint |
| Changing a field's type | Adding a new optional query parameter |
| Removing an endpoint | Adding a new enum value (if clients handle unknown) |
| Making an optional field required | Improving error messages |

## Rate Limiting

Include rate-limit headers in every response so clients can self-regulate:

```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 742
X-RateLimit-Reset: 1702483200
```

| Header | Description |
|--------|-------------|
| `X-RateLimit-Limit` | Max requests allowed in the window |
| `X-RateLimit-Remaining` | Requests remaining in current window |
| `X-RateLimit-Reset` | Unix timestamp when the window resets |

When the limit is exceeded, return:

```json
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Too many requests. Retry after 30 seconds.",
    "details": {
      "retry_after": 30
    }
  }
}
```

Return `429 Too Many Requests` with a `Retry-After` header (in seconds).

## Anti-Patterns

Avoid these. When you see them in a codebase, flag or refactor.

| Anti-Pattern | Problem | Do Instead |
|-------------|---------|-----------|
| Verbs in URLs | `/getUsers`, `/deleteOrder` | Use HTTP methods: `GET /users`, `DELETE /orders/:id` |
| Deep nesting (3+ levels) | `/users/:id/orders/:oid/items/:iid/reviews` | Flatten: `/order-items/:iid/reviews` |
| 200 for errors | Client can't distinguish success from failure by status | Use appropriate 4xx/5xx codes |
| GET for mutations | Breaks caching, prefetching, crawlers | Use POST, PUT, PATCH, DELETE |
| Plural/singular inconsistency | `/user/1` but `/orders` | Always use plural: `/users/1`, `/orders` |
| Returning arrays at root | Vulnerable to JSON hijacking, can't add metadata | Wrap in `{ "data": [...] }` |
| Leaking internal errors | Stack traces, SQL errors in responses | Return generic message, log details server-side |
| Ignoring `Accept` headers | Client can't negotiate content type | Respect `Accept` or return `406` |
| Custom status codes | Clients won't understand them | Stick to standard HTTP status codes |
| Inconsistent error shapes | Some errors return `{ message }`, others `{ error }` | Use one error shape everywhere |
