---
name: retro
description: Run a structured retrospective for a sprint, launch, or project. Generates actionable insights from what worked and what didn't. Use when user says "run a retro", "retrospective", "post-mortem", "what went wrong", "lessons learned", "sprint review", or needs to reflect on past work and improve.
user-invokable: true
args:
  - name: target
    description: The sprint, launch, or project to retrospect on (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

Run a retrospective that produces action items, not just observations. A retro without specific next steps is a venting session.

**Principles — hold these throughout:**

- **Blame-free.** Focus on systems, processes, and circumstances — not people. "The deploy process let a bug through" not "Bob broke prod."
- **Actions over observations.** Every theme must produce a concrete change. If it doesn't lead to action, note it and move on.
- **Max 3 action items.** More than three won't get done. Prioritize ruthlessly.

## Step 1: Set Context

Establish the boundaries before gathering input:

| Question | Answer |
|----------|--------|
| **What period/project?** | Name it specifically — "Sprint 14" or "Billing v2 launch" |
| **What was the goal?** | What were we trying to accomplish? |
| **What actually happened?** | State the outcome factually — shipped on time, missed by 2 weeks, hit metrics, didn't |
| **Who was involved?** | List the team so contributions are visible |
| **Timeline** | Start date, end date, key milestones |

Write a 2-3 sentence summary: "We set out to [goal]. We [what happened]. The result was [outcome]."

If the user hasn't provided this context, ask for it. Don't fabricate project history.

## Step 2: Gather Input — The 4Ls

Use the 4Ls framework to collect observations. Ask the user to contribute to each category:

| Category | Prompt | What goes here |
|----------|--------|----------------|
| **Liked** | What went well? What should we keep doing? | Practices, decisions, or moments that worked |
| **Learned** | What did we discover? What surprised us? | New knowledge, skills, or insights gained |
| **Lacked** | What was missing? What held us back? | Resources, information, tools, or processes we needed but didn't have |
| **Longed For** | What do we wish we had? What would make next time better? | Improvements, tools, practices, or changes we want |

**Gathering tips:**

- Ask for specifics, not generalities. "Liked: good communication" is useless. "Liked: daily 10-min standups kept everyone unblocked" is actionable.
- Aim for 3-5 items per category.
- If working async, give people a template to fill in before the session.

## Step 3: Identify Themes

Group related items across the 4Ls. Look for patterns:

1. **Cluster related items.** If three people mention deployment issues, that's a theme.
2. **Name each theme clearly.** "Deployment reliability" not "various issues."
3. **Find root causes.** For the biggest themes, use the 5 Whys:

```
Problem: Feature launched 2 weeks late.
Why? → Integration testing took longer than expected.
Why? → We discovered API incompatibilities late.
Why? → No integration test environment existed.
Why? → We hadn't invested in test infrastructure for this service.
Why? → Test infra wasn't prioritized because the team was focused on feature work.

Root cause: No dedicated time for test infrastructure investment.
```

Stop when you reach something the team can actually change. Don't go deeper than useful.

4. **Rank themes by impact.** Which ones, if fixed, would make the biggest difference next time?

## Step 4: Generate Action Items

For each of the top 2-3 themes, create one action item. Every action must be:

| Requirement | Example |
|-------------|---------|
| **Specific** | "Set up staging environment with seed data" — not "improve testing" |
| **Assigned** | One owner, by name or role. "The team" owns nothing. |
| **Time-bound** | "Complete by end of Sprint 15" — not "soon" |

```
ACTION ITEM TEMPLATE:

Theme: [theme name]
Action: [specific thing to do]
Owner: [person or role]
Due: [date or sprint]
Success looks like: [how we know it's done]
```

**Hard limit: 3 action items.** If you have 5 good ones, pick the 3 with highest impact. Carry the rest to next retro if they're still relevant.

## Step 5: Write the Retro Summary

Produce a single document the team can reference:

```
# Retro: [Project/Sprint Name]

**Period:** [start] — [end]
**Team:** [names/roles]
**Goal:** [what we set out to do]
**Outcome:** [what actually happened]

## Summary
[2-3 sentences: goal, what happened, result]

## 4Ls

### Liked
- [item]
- [item]

### Learned
- [item]
- [item]

### Lacked
- [item]
- [item]

### Longed For
- [item]
- [item]

## Themes
1. **[Theme name]** — [1-sentence description]. Root cause: [root cause].
2. **[Theme name]** — [1-sentence description]. Root cause: [root cause].
3. **[Theme name]** — [1-sentence description]. Root cause: [root cause].

## Action Items

| # | Action | Owner | Due | Done? |
|---|--------|-------|-----|-------|
| 1 | [specific action] | [owner] | [date] | [ ] |
| 2 | [specific action] | [owner] | [date] | [ ] |
| 3 | [specific action] | [owner] | [date] | [ ] |

## Notes
[Anything else worth recording that doesn't fit above]
```

## Output

Deliver the retro summary using the template above. Customize it to the actual project — don't leave placeholder text. If information is missing, flag what you need from the user rather than filling in generic content.
