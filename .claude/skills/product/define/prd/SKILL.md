---
name: prd
description: Write a product requirements document from context. Covers problem statement, goals, scope, user stories, success criteria, and edge cases. Use when user says "write a PRD", "product requirements", "requirements doc", "spec this feature", "document requirements", or needs to define what to build and why.
user-invokable: true
args:
  - name: target
    description: The feature or product to write requirements for (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

# Product Requirements Document

Write a PRD that answers two questions clearly: **what are we building** and **why does it matter**. Everything else is supporting detail.

## Step 1: Gather Context

Before writing anything, nail down these three inputs. If the user hasn't provided them, ask.

| Input | Question | Watch out for |
|-------|----------|---------------|
| **Problem** | What pain exists today? Who feels it? | Jumping to solutions before the problem is sharp |
| **Audience** | Who specifically benefits? What's their situation? | "Everyone" is not an audience |
| **Current state** | How do people solve this now? What's broken? | Assuming nothing exists today |

If a JTBD analysis, research, or prior conversations exist, pull from them. Don't invent context.

## Step 2: Write the PRD

Use this structure. Every section earns its place — skip a section only if it genuinely doesn't apply.

```
# PRD: [Feature / Product Name]

## Problem Statement
What's broken, missing, or painful. Ground it in real user behavior.
Who has this problem. How often. How bad.

## Goals & Non-Goals

### Goals
- [ ] Measurable outcome this work achieves
- [ ] Another measurable outcome

### Non-Goals
- What this work deliberately does NOT try to solve (and why)

## User Stories / Job Stories
Use job story format when possible:
- When [situation], I want to [motivation], so I can [outcome]

Keep to 3-8 stories. If you have more, the scope is too big.

## Functional Requirements
What the system must do. Number them for traceability.

FR-1: [Requirement]
FR-2: [Requirement]

## Non-Functional Requirements
Performance, security, accessibility, scale, compatibility.

NFR-1: [Requirement]
NFR-2: [Requirement]

## Success Metrics
How we'll know this worked. Be specific:
- Metric + target + timeframe
- e.g., "Reduce average onboarding time from 12 min to under 5 min within 30 days of launch"

## Open Questions
What's unresolved. Who owns answering it. When it needs an answer by.

| # | Question | Owner | Deadline |
|---|----------|-------|----------|

## Out of Scope
What's explicitly excluded from this effort. Prevents scope creep later.
```

## Quality Checks

Run these before delivering the PRD:

- **Every requirement is testable.** If you can't write a pass/fail check for it, rewrite it. "Fast" is not testable. "Loads in under 2 seconds on 3G" is.
- **No solutions hiding as requirements.** "Use Redis for caching" is a solution. "Cache frequently accessed data with sub-100ms retrieval" is a requirement.
- **Scope is realistic.** If the PRD has 30+ requirements, it's a roadmap pretending to be a PRD. Split it.
- **Goals are measurable.** Each goal should have a number attached or a clear yes/no test.
- **Non-goals are deliberate.** They prevent future arguments about what "should have been included."
- **Open questions have owners.** An open question without an owner is a stalled decision.

## Tone and Format

- Write requirements as imperative statements: "The system shall..." or "Users can..."
- Use numbered identifiers (FR-1, NFR-1) so engineers and designers can reference them
- Keep the language concrete. Replace "intuitive" with observable behavior. Replace "scalable" with specific numbers.
- One requirement per line. Compound requirements hide complexity.

## When the PRD Is Done

Hand it off with three things clear:
1. **What's decided** — requirements that are locked
2. **What's open** — questions that still need answers
3. **What's next** — who reviews, who builds, what's the timeline
