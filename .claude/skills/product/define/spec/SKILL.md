---
name: spec
description: Write a technical specification or acceptance criteria from a PRD or user stories. Bridges product requirements to engineering implementation. Use when user says "write a tech spec", "technical specification", "acceptance criteria", "engineering spec", "how should we build this", or needs to translate product requirements into technical plans.
user-invokable: true
args:
  - name: target
    description: The PRD, stories, or feature to spec (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

# Technical Specification

A tech spec answers **how** we'll build something — without over-prescribing implementation details that engineers should own. It bridges product requirements to engineering execution.

## Step 1: Read the Requirements

Before writing, gather the inputs. A spec without requirements is guesswork.

| Source | What to pull from it |
|--------|---------------------|
| **PRD** | Problem, goals, functional requirements, non-functional requirements, scope boundaries |
| **Job stories** | User workflows, acceptance criteria, edge cases |
| **Conversation** | If neither exists, interview the user to establish what the system must do and for whom |

Flag anything ambiguous. A spec built on vague requirements produces vague engineering.

## Step 2: Write the Spec

Use this structure. Adapt headings to the project — not every section applies every time.

```
# Tech Spec: [Feature / System Name]

## Overview
One paragraph. What this is, why we're building it, and what it connects to.
Link to the PRD or source requirements.

## Technical Approach
The core design decision. How this fits into the existing system.
Keep it to the architecture level — components, their responsibilities,
how they interact. Don't write pseudocode unless it clarifies a tricky algorithm.

## Data Model
New tables, fields, or schema changes. Include:
- Field name, type, constraints
- Relationships to existing models
- Migration notes (new table vs altering existing)

## API Design
Endpoints, methods, request/response shapes.
For each endpoint:
- Method + path
- Request body / params
- Response shape
- Error cases

## Edge Cases
What happens when things go wrong or get weird.

| Case | Behavior | Rationale |
|------|----------|-----------|
| User submits duplicate | Return existing record, don't create new | Prevents data bloat |
| Payload exceeds size limit | Reject with 413, log for monitoring | Protects downstream services |

## Testing Strategy
What gets tested and how.
- Unit tests: which modules, what coverage target
- Integration tests: which workflows end-to-end
- Manual QA: what can't be automated and why

## Migration Plan
Only if changing existing systems. Cover:
- Data migration steps
- Backward compatibility (can old and new coexist during rollout?)
- Rollback procedure

## Rollout Plan
How this gets to users.
- Feature flag strategy
- Phased rollout stages (internal → beta → GA)
- Monitoring and alerting during rollout
- Success/failure criteria for each stage
```

## Decision Log

Every non-obvious choice gets logged. This prevents re-litigating decisions later and helps future engineers understand why things are the way they are.

```
## Decisions

### D-1: [Short description of the decision]
**Options considered:**
1. Option A — [brief description]
2. Option B — [brief description]
3. Option C — [brief description]

**Decision:** Option B

**Rationale:** [Why this option wins. Reference constraints, trade-offs,
or requirements that drove the choice.]
```

Keep decisions numbered (D-1, D-2) so they can be referenced in code comments and PR descriptions.

## Quality Checks

Before delivering the spec, verify:

- **Does it answer "how" without dictating unnecessary "what exactly"?** The spec should constrain the architecture, not micromanage the implementation. Engineers need room to make local decisions.
- **Are edge cases covered?** If you only describe the happy path, the implementation will only handle the happy path.
- **Is the data model complete?** Missing a field or relationship here means a migration later.
- **Do API contracts match the requirements?** Walk each job story through the API. Does the data flow support every acceptance criterion?
- **Is the testing strategy proportional?** Critical paths get thorough tests. Low-risk utilities get basic coverage. Don't spec 100% coverage everywhere.
- **Is the rollout reversible?** If something goes wrong in production, can you roll back without data loss?

## Scope Boundaries

A tech spec is not:
- **A tutorial.** Don't explain how frameworks work. Assume engineering competence.
- **A PRD.** Don't redefine what we're building. Reference the PRD and focus on how.
- **A project plan.** Don't include timelines or task assignments. That's a separate artifact.

## When the Spec Is Done

Ship it with:
1. **Clear status** — Draft, In Review, or Approved
2. **Open questions** — What still needs answers before building starts
3. **Review ask** — Who should review (engineering, security, platform) and what feedback you need
