---
name: code-review
description: Perform a structured code review covering correctness, readability, performance, security, and maintainability. Use when user says "review this code", "code review", "check this PR", "review my changes", "is this code good", or needs expert feedback on code quality.
user-invokable: true
args:
  - name: target
    description: The file, directory, or PR to review (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

Review code across five dimensions and produce a prioritized list of findings. Suggest improvements; don't demand them. Praise good patterns alongside issues.

## Step 1: Read the Code

Understand intent before judging implementation. Before writing a single finding:

- Identify the purpose of the change (feature, fix, refactor, test).
- Note the architectural context (framework, patterns, conventions in use).
- Read related tests and documentation if available.
- If reviewing a PR, read the description and linked issues first.

## Step 2: Review Across Five Dimensions

Evaluate the code against each dimension using the specific checks below.

### Correctness

| Check | What to look for |
|-------|-----------------|
| Logic errors | Incorrect conditionals, wrong operator precedence, inverted boolean logic |
| Off-by-one | Fence-post errors in loops, slices, pagination, boundary conditions |
| Null / undefined handling | Missing optional chaining, unchecked nullable returns, empty-array assumptions |
| Race conditions | Shared mutable state, unguarded async sequences, stale closures |
| Error paths | Swallowed exceptions, missing try/catch, unhelpful error messages, no fallback |
| Edge cases | Empty inputs, very large inputs, unicode, negative numbers, timezone boundaries |

### Readability

| Check | What to look for |
|-------|-----------------|
| Naming | Ambiguous variable names, misleading function names, inconsistent conventions |
| Function length | Functions doing more than one thing, > 40 lines is a smell |
| Nesting depth | More than 3 levels of nesting; consider early returns or extraction |
| Comments | Missing "why" comments on non-obvious logic; remove comments that restate code |
| Consistency | Style deviations from the rest of the codebase |
| Cognitive load | Complex ternaries, implicit type coercion, magic numbers or strings |

### Performance

| Check | What to look for |
|-------|-----------------|
| Unnecessary re-renders | Missing memoization, inline object/function creation in render path |
| N+1 queries | Database calls inside loops, unbatched API requests |
| Missing memoization | Expensive computations recalculated on every render or call |
| Large bundles | Heavy imports that could be lazy-loaded or replaced with lighter alternatives |
| Algorithmic complexity | O(n^2) or worse where O(n) or O(n log n) is achievable |
| Memory leaks | Uncleared intervals/timeouts, missing event listener cleanup, growing caches |

### Security

| Check | What to look for |
|-------|-----------------|
| Injection | Unsanitized user input in SQL, shell commands, templates, or regex |
| XSS | Unescaped output, dangerouslySetInnerHTML, innerHTML without sanitization |
| Auth bypass | Missing authorization checks, client-side-only access control |
| Secrets in code | API keys, tokens, passwords committed or logged |
| Unsafe deserialization | Parsing untrusted JSON/YAML without validation, eval of user input |
| CSRF / CORS | Missing CSRF tokens, overly permissive CORS configuration |

### Maintainability

| Check | What to look for |
|-------|-----------------|
| Coupling | Components tightly bound to implementation details of other modules |
| Duplication | Same logic repeated in multiple places; candidate for extraction |
| Test coverage | Untested happy paths, missing edge-case tests, brittle test setup |
| Dead code | Unused exports, commented-out blocks, unreachable branches |
| Unclear abstractions | Over-abstraction (unnecessary indirection) or under-abstraction (god functions) |
| Upgrade path | Deprecated APIs, framework-version-specific hacks, pinned dependencies |

## Step 3: Output as a Prioritized Finding List

Organize every finding by severity using the classification table below.

### Severity Classification

| Severity | Qualifies when... | Examples |
|----------|-------------------|----------|
| **Critical** | Causes data loss, security breach, or crash in production | SQL injection, unhandled null on critical path, race condition corrupting state |
| **High** | Breaks functionality or significantly degrades UX | Logic error in business rule, missing error handling on API call, auth check skipped |
| **Medium** | Impacts quality but doesn't break things | N+1 query on secondary page, moderate duplication, missing test for edge case |
| **Low** | Minor improvement opportunity | Slightly better naming, small refactor for clarity, non-critical performance tweak |
| **Nit** | Stylistic or trivial; optional to address | Extra whitespace, import order, comment wording, personal preference |

### Finding Format

For each finding, include:

- **Severity**: Critical / High / Medium / Low / Nit
- **Dimension**: Which of the five dimensions it falls under
- **Location**: File path and line number (or range)
- **Issue**: What the problem is (one sentence)
- **Suggestion**: How to improve it (concrete, with code snippet when helpful)
- **Why it matters**: Impact on users, developers, or the system

### Summary Section

After all findings, include:

- **Total count** by severity
- **Top 3 priorities** to address first
- **Positive patterns** observed (what the code does well)
- **Overall assessment** (one paragraph)

## Tone Guidance

- Use "Consider X" not "You should X."
- Use "This could Y" not "This will Y" unless certain.
- Praise good patterns: "Nice use of early returns here" or "Good separation of concerns."
- Explain the "why" for every suggestion; reviewees learn more from reasoning than rules.
- Acknowledge trade-offs: "This adds complexity but improves testability."
- When uncertain, say so: "I'm not sure about the intent here — if Z is the goal, then consider W."

**NEVER**:
- Nitpick style when there are critical issues (prioritize ruthlessly)
- Flag something without a concrete suggestion for improvement
- Assume malice or carelessness — the author may have context you don't
- Rewrite entire functions in suggestions — keep diffs small and focused
