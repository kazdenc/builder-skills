---
name: competitive
description: Structured competitive analysis with feature comparison, positioning maps, and gap identification. Use when user says "competitive analysis", "analyze competitors", "compare products", "market landscape", "who are our competitors", or needs to understand competitive positioning.
user-invokable: true
args:
  - name: target
    description: The product, market, or competitors to analyze (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

# Competitive Analysis

Work through these five steps in order. Each builds on the previous one.

---

## Step 1 — Identify Competitors

Sort every competitor into one of three buckets:

| Type | Definition | How to find them |
|------|-----------|-----------------|
| **Direct** | Solves the same job with a similar product | Search the category name, check review sites, ask users "what else did you consider?" |
| **Indirect** | Solves the same job with a different approach | Ask "how did people do this before products like ours existed?" |
| **Substitutes** | Competes for the same budget, time, or attention | Ask "what would they do instead of solving this job at all?" |

Include "do nothing" as a substitute — it is always the strongest competitor.

List 3–7 competitors. More than that dilutes focus.

## Step 2 — Feature Comparison Matrix

Build a table. Rows are capabilities that matter to the job. Columns are competitors. Score each cell.

```
| Capability              | Us | Competitor A | Competitor B | Competitor C |
|------------------------|----|-------------|-------------|-------------|
| [capability 1]         | .. | ..          | ..          | ..          |
| [capability 2]         | .. | ..          | ..          | ..          |
| ...                    |    |             |             |             |
```

Scoring:
- **Strong** — does this well, customers recognize it as a strength
- **Adequate** — does it, no complaints, no praise
- **Weak** — does it poorly or partially
- **Missing** — doesn't do it at all

Pick capabilities from the user's perspective, not the feature list. "Quickly see what changed since yesterday" beats "activity feed" or "notification system."

## Step 3 — Positioning Analysis

Plot competitors along the two dimensions that matter most for this market. Pick dimensions from the job, not from product attributes.

```
                        [Dimension A — high]
                              │
                              │    ○ Competitor B
                              │
  [Dimension B — low] ───────┼──────── [Dimension B — high]
                              │
              ○ Competitor A  │
                              │         ○ Competitor C
                        [Dimension A — low]
```

Call out:
- **Clusters** — where multiple competitors sit on top of each other (crowded positioning)
- **Gaps** — empty quadrants or open space (potential opportunity)
- **Movement** — where competitors are heading based on recent releases or messaging shifts

## Step 4 — Strengths and Weaknesses by Job

For each competitor, answer two questions through the lens of the jobs customers hire them for:

| Competitor | Jobs served well | Jobs served poorly |
|-----------|-----------------|-------------------|
| Competitor A | [list jobs where they excel] | [list jobs where they fall short] |
| Competitor B | ... | ... |

Don't assess competitors against your priorities — assess them against what their own customers hire them to do. A competitor can be "weak" at something that doesn't matter to their audience.

## Step 5 — Opportunities

Synthesize the previous steps into a short list of opportunities. Each opportunity should be:

| Opportunity | Evidence | Who it serves | Risk |
|------------|----------|--------------|------|
| [Underserved segment or unmet need] | [Which steps above revealed it] | [Who benefits] | [What could go wrong] |

Look for:
- **Underserved segments** — groups whose job is poorly done by every current option
- **Unmet needs** — specific needs in the job that no competitor addresses well
- **Over-served segments** — groups paying for capabilities they don't need (room for a simpler/cheaper entry)
- **Shifting context** — changes in technology, regulation, or behavior that invalidate current positioning

---

## Output Format

Deliver the analysis as a single structured document containing:
1. Competitor list (with type labels)
2. Feature comparison table
3. Positioning map (text diagram)
4. JTBD strengths/weaknesses table
5. Ranked opportunity list

Keep each section tight. The value is in the structure and the gaps it reveals, not in lengthy descriptions of what each competitor does.
