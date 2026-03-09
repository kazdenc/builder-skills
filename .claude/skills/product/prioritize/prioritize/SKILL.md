---
name: prioritize
description: Score and rank features or initiatives using proven frameworks like RICE, opportunity scoring, or impact/effort. Use when user says "prioritize features", "what should we build first", "rank these ideas", "RICE scoring", "priority matrix", "stack rank", or needs to make data-informed build decisions.
user-invokable: true
args:
  - name: target
    description: The features, backlog, or ideas to prioritize (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

Score and rank features or initiatives so teams build the highest-value things first.

## Step 1: Gather the List

Collect the items to prioritize. If the user hasn't provided them, ask. Each item needs a clear, concise name and a one-sentence description of what it delivers.

Don't prioritize fewer than 3 items (just pick the best one) or more than 25 (split into themes first, then prioritize within each).

## Step 2: Choose a Framework

Pick one based on what the team actually has available:

| If you have... | Use | Why |
|----------------|-----|-----|
| Usage data, reach estimates, engineering sizing | **RICE** | Rigorous, accounts for confidence in your estimates |
| JTBD need statements with importance/satisfaction data | **Opportunity scoring** | Finds underserved needs directly |
| A large list and need a quick first pass | **Impact/Effort matrix** | Fast triage, no math required |
| Limited data but reasonable intuition | **ICE** | Lighter-weight RICE, moves fast |

If unsure, default to **Impact/Effort matrix** for triage, then apply **RICE** to the top-right quadrant.

## Step 3: Score Each Item

### RICE

Score each item on four dimensions, then calculate:

```
RICE Score = (Reach x Impact x Confidence) / Effort
```

| Dimension | What it measures | Scale |
|-----------|-----------------|-------|
| **Reach** | How many users/accounts affected per quarter | Actual number (e.g., 500 users/quarter) |
| **Impact** | How much this moves the needle per user | 3 = massive, 2 = high, 1 = medium, 0.5 = low, 0.25 = minimal |
| **Confidence** | How sure you are about the above estimates | 100% = high, 80% = medium, 50% = low. Below 50% means you need research, not prioritization |
| **Effort** | Person-months of work | Actual estimate (e.g., 2 person-months) |

### Opportunity Scoring

For each need identified through JTBD work:

```
Opportunity Score = Importance + max(Importance - Satisfaction, 0)
```

| Dimension | Scale |
|-----------|-------|
| **Importance** | 1-10: How important is this need to the job performer? |
| **Satisfaction** | 1-10: How well do current solutions satisfy this need? |

Scores above 12 are strong opportunities. Scores above 15 are urgent.

### Impact/Effort Matrix

Plot each item on a 2x2:

```
          High Impact
               |
   Quick Wins  |  Big Bets
   DO FIRST    |  PLAN CAREFULLY
  -------------|-------------
   Fill-ins    |  Money Pits
   DO LAST     |  DON'T DO
               |
          Low Impact
  Low Effort ------- High Effort
```

Score impact 1-5 and effort 1-5 if you want numbers. Otherwise, just place items on the grid.

### ICE

```
ICE Score = Impact x Confidence x Ease
```

| Dimension | Scale |
|-----------|-------|
| **Impact** | 1-10: How much will this move the target metric? |
| **Confidence** | 1-10: How sure are you about impact and ease? |
| **Ease** | 1-10: How easy is this to implement? (10 = trivial, 1 = months of work) |

## Step 4: Rank and Present

Sort items by score descending. Present as a table:

```markdown
| Rank | Item | Score | R | I | C | E | Rationale |
|------|------|-------|---|---|---|---|-----------|
| 1    | ...  | ...   | . | . | . | . | ...       |
| 2    | ...  | ...   | . | . | . | . | ...       |
```

Adapt columns to match the chosen framework. Always include the final score, component scores, and a brief rationale for each item.

Group results into tiers when scores cluster:
- **Tier 1** — Build now. Clear leaders.
- **Tier 2** — Build next. Strong but not urgent.
- **Tier 3** — Build later or revisit. Low scores or low confidence.

## Step 5: Sanity Check

After ranking, review the output critically:

- **Does the top item actually feel like the top priority?** If not, identify what the framework missed (strategic alignment, dependencies, political reality).
- **Are there items with low confidence dragging down or inflating scores?** Flag them separately as "needs research before prioritizing."
- **Do dependencies change the order?** Item #3 might need to ship before Item #1 can work.
- **Is there a quick win in the top 5 that could ship this week?** Call it out. Momentum matters.

When framework output and intuition disagree, don't just override the numbers. Name the disagreement explicitly: "RICE ranks X first, but the team believes Y is more urgent because [reason]." Then decide deliberately.

## Output Format

Deliver:
1. The framework chosen and why
2. A scored, ranked table
3. Tier groupings with recommended action (build now / build next / revisit)
4. Any flags: low-confidence items, dependency issues, intuition mismatches
