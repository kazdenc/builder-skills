---
name: metrics
description: Define success metrics and KPIs for a feature or product. Identifies leading and lagging indicators with instrumentation guidance. Use when user says "define metrics", "KPIs", "how do we measure success", "success criteria", "what should we track", "instrumentation plan", or needs to quantify outcomes.
user-invokable: true
args:
  - name: target
    description: The feature or product to define metrics for (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

Define what success looks like in numbers. Work through each step — don't jump to a list of metrics without grounding them in the goal first.

## Step 1: Identify the Goal

Before picking metrics, answer:

- **What job does this feature serve?** What progress is the user trying to make? (Reference the jtbd skill if available.)
- **What outcome matters to the business?** Revenue, retention, efficiency, expansion — pick one.
- **What behavior change are you expecting?** Users should do more of X, less of Y, start doing Z.

If you can't state the goal in one sentence, the metrics will be unfocused. Push for clarity.

## Step 2: Define a Primary Metric

Pick the ONE number that best represents success. This is the metric the team checks every morning.

**How to choose — the 3 A's:**

| Test | Question | Bad example | Good example |
|------|----------|------------|--------------|
| **Actionable** | Can the team change this number with their work? | Total registered users (too broad) | Weekly active users of this feature |
| **Accessible** | Can everyone on the team understand and check it? | Custom composite score | % of users who complete core action |
| **Auditable** | Can you verify the data is correct? | Self-reported satisfaction | Event-tracked completion rate |

**Common primary metrics by goal:**

| Goal | Primary metric |
|------|---------------|
| Adoption | % of eligible users who use the feature in first 7 days |
| Engagement | Weekly active usage (sessions, actions, or time) |
| Retention | % still using after 30 days |
| Efficiency | Time to complete task (before vs. after) |
| Revenue | Conversion rate or revenue per user |
| Quality | Error rate or task success rate |

State it precisely: "% of users who create at least one invoice within 7 days of first seeing the feature" — not "adoption."

## Step 3: Define Supporting Metrics

### Leading indicators

Predict future success. Move before the primary metric does. Use these to course-correct early.

| Example | What it predicts |
|---------|-----------------|
| Activation rate (completed setup) | Future retention |
| Feature discovery rate (saw the entry point) | Future adoption |
| Time to first value | Future engagement |
| Onboarding completion | Future active usage |

### Lagging indicators

Confirm past success. Move slowly. Use these to validate that early signals were real.

| Example | What it confirms |
|---------|-----------------|
| 30/60/90-day retention | Sustained value |
| Net revenue retention | Business impact |
| NPS / CSAT change | User sentiment shift |
| Support ticket volume (down) | Reduced friction |

### Guardrail metrics

Things that should NOT get worse when the feature launches. Set these before shipping.

| Guardrail | Threshold |
|-----------|-----------|
| Page load time | Must stay under current p95 |
| Error rate | Must not increase >0.1% |
| Existing feature usage | Must not drop (cannibalization check) |
| Support ticket volume (up) | Spike is okay for 1 week, not 4 |
| Unsubscribe rate | Must not increase after announcement emails |

If a guardrail breaks, investigate before celebrating the primary metric.

## Step 4: Anti-Metrics — What NOT to Track

Vanity metrics feel good but don't inform decisions. Ignore these:

| Vanity metric | Why it's misleading | Track this instead |
|---------------|--------------------|--------------------|
| Total signups (all time) | Only goes up, tells you nothing | Weekly new active users |
| Page views | Traffic without context | Conversion rate from page |
| Total features shipped | Output, not outcome | Adoption rate per feature |
| App downloads | Acquisition without activation | Day-7 retention |
| Time on page (alone) | Could mean engaged or confused | Task completion rate |

**Rule of thumb:** If a metric can only go up and never triggers action, it's vanity.

## Step 5: Instrumentation Plan

Metrics are useless without reliable tracking. For each metric, define:

| Event name | Trigger | Properties | Where tracked |
|------------|---------|------------|---------------|
| `feature_viewed` | User sees the feature entry point | `user_id`, `source`, `timestamp` | Analytics (e.g., Amplitude, Mixpanel) |
| `feature_activated` | User completes core action for first time | `user_id`, `time_to_activate`, `timestamp` | Analytics |
| `feature_used` | User completes core action (any time) | `user_id`, `session_id`, `timestamp` | Analytics |
| `feature_error` | Something fails | `user_id`, `error_type`, `timestamp` | Error tracking (e.g., Sentry) |

**Instrumentation checklist:**

- [ ] Events fire in both frontend and backend where appropriate
- [ ] User ID is attached to all events (for cohort analysis)
- [ ] Timestamps are consistent (UTC)
- [ ] Properties include enough context to segment (source, plan, cohort)
- [ ] Events are tested in staging before launch
- [ ] Dashboard or report is built before launch day
- [ ] Team knows where to check metrics and how often

## Metrics Template

Use this table as the deliverable:

```
| Metric | Type | Target | How to Measure |
|--------|------|--------|----------------|
| [Primary metric] | Primary | [specific number] | [event/query] |
| [Leading indicator 1] | Leading | [specific number] | [event/query] |
| [Leading indicator 2] | Leading | [specific number] | [event/query] |
| [Lagging indicator] | Lagging | [specific number] | [event/query] |
| [Guardrail 1] | Guardrail | [must not exceed X] | [event/query] |
| [Guardrail 2] | Guardrail | [must not exceed X] | [event/query] |
```

## If You Can Only Track 3 Things

When instrumentation budget is tight, track exactly these:

1. **Adoption** — did users find and try it? (`feature_activated` / eligible users)
2. **Completion** — did they succeed at the core task? (`task_completed` / `task_started`)
3. **Return** — did they come back? (`feature_used` where `usage_count > 1` within 14 days)

These three form a funnel: discovered it, got value, came back. Everything else is refinement.

## Output

Deliver the metrics plan as a document with:

1. **Goal** — one sentence
2. **Primary metric** — name, definition, target
3. **Supporting metrics** — leading, lagging, guardrails in table format
4. **Instrumentation plan** — events, properties, where tracked
5. **Anti-metrics** — what you're deliberately not tracking and why
