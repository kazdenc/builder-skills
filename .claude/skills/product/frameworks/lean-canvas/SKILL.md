---
name: lean-canvas
description: One-page business model framework for rapid validation and strategic clarity. Use when user needs to validate a business idea, map out a product strategy, or evaluate product-market fit. Works for new products, pivots, and feature-level business cases. Trigger phrases include "lean canvas", "business model", "one-page plan", "validate this idea", "product-market fit", "business case".
metadata:
  author: builder-skills
  version: "1.0.0"
---

# Lean Canvas

When someone asks you to think through a business model, product strategy, or idea validation, use the lean canvas. Fill one page, not a slide deck. The constraint is the point — it forces clarity.

A lean canvas is a snapshot of a business model hypothesis. Every box is a bet. Treat it that way: state the bet, then figure out how to test it.

## The Canvas Layout

```
┌───────────────┬───────────────┬───────────────┬───────────────┬───────────────┐
│               │               │               │               │               │
│   PROBLEM     │   SOLUTION    │   UNIQUE      │   UNFAIR      │   CUSTOMER    │
│               │               │   VALUE       │   ADVANTAGE   │   SEGMENTS    │
│   Top 3       │   Top 3       │   PROPOSITION │               │               │
│   problems    │   features    │               │   Can't be    │   Target      │
│               │               │   Single,     │   easily      │   customers   │
│               │               │   clear,      │   copied or   │               │
│               │               │   compelling  │   bought      │   Early       │
│               │               │   message     │               │   adopters    │
│               │               │               │               │               │
├───────────────┴───────┬───────┴───────────────┴───────┬───────┴───────────────┤
│                       │                               │                       │
│   KEY METRICS         │   CHANNELS                    │   REVENUE STREAMS     │
│                       │                               │                       │
│   Key activities      │   Path to                     │   Revenue model       │
│   you measure         │   customers                   │   Pricing             │
│                       │                               │                       │
├───────────────────────┴───────────────────────────────┴───────────────────────┤
│                               │                                               │
│   COST STRUCTURE              │   REVENUE STREAMS                             │
│                               │                                               │
│   Fixed and variable costs    │   (continued from above)                      │
│                               │                                               │
└───────────────────────────────┴───────────────────────────────────────────────┘
```

Simplified reading order — fill boxes in this sequence for the clearest thinking:

```
1. Customer Segments  →  2. Problem  →  3. Unique Value Proposition
4. Solution  →  5. Channels  →  6. Revenue Streams
7. Cost Structure  →  8. Key Metrics  →  9. Unfair Advantage
```

Start with the customer, not the solution. If someone hands you a solution first, work backward to the customer and problem before filling in anything else.

## The Nine Boxes

### 1. Customer Segments

Put the specific group of people who have the problem. Not "everyone." Not "businesses." Name a narrow early-adopter segment you can reach first.

**Fill with:** One primary segment. Optionally one secondary. Describe them by behavior, not demographics. "Freelance designers who manage their own invoicing" beats "millennials aged 25-35."

**Common mistakes:**
- Too broad ("SMBs," "developers"). Narrow it until you can picture where they hang out online.
- Listing multiple unrelated segments. One canvas = one segment. Make separate canvases for different segments.
- Confusing users with customers (the person who pays may differ from the person who uses).

**Connects to:** Problem (their problem, not yours), Channels (where you find them), Revenue (what they'll pay).

### 2. Problem

List the top 1-3 problems this segment actually has. Not problems you wish they had. Not problems your solution happens to solve.

**Fill with:** Concrete problems stated from the customer's perspective. "I spend 4 hours a week chasing late invoices" not "invoicing is inefficient." Also note existing alternatives — how they solve this problem today (even badly).

**Common mistakes:**
- Listing problems only your solution solves (solution bias). Step back: what's the real problem?
- Skipping existing alternatives. If you don't know how they cope today, you don't understand the problem.
- Too abstract. "Communication is hard" is not a problem — "remote teams lose context when handoffs happen across time zones" is.

**Connects to:** Customer Segments (whose problem), Solution (your answer), UVP (why your answer is better than existing alternatives).

### 3. Unique Value Proposition

Write a single, clear sentence that says why this product deserves attention. This is the hardest box. Don't rush it.

**Fill with:** One sentence. Format: `[End result customer wants] + [Specific time period or qualifier] + [Address the main objection]`. Example: "Get paid on time, every time — without chasing clients."

**Common mistakes:**
- Feature soup ("AI-powered, cloud-native, real-time collaboration platform"). Features go in Solution.
- No differentiation. If you can swap in a competitor's name and it still works, it's not a UVP.
- Too clever. Clarity beats cleverness. The reader should understand it in under 5 seconds.

**Connects to:** Problem (the pain it resolves), Solution (what delivers it), Customer Segments (who cares about this promise).

### 4. Solution

List the top 3 features or capabilities that deliver on the UVP. Keep it minimal — this is your MVP scope.

**Fill with:** Three bullet points max. Each should map directly to a problem from box 2. If a feature doesn't map to a stated problem, cut it.

**Common mistakes:**
- Feature overload. More than 3 means you haven't prioritized.
- Solutions that don't map back to stated problems.
- Building before validating. The solution box is a hypothesis — test it before you commit.

**Connects to:** Problem (each solution maps to a problem), UVP (together they deliver the promise), Cost Structure (what it costs to build and run).

### 5. Channels

Describe how you reach and deliver value to customers. Split into two phases: how they find out about you, and how they get the product.

**Fill with:** Specific channels. "Content marketing" is vague. "SEO-driven blog targeting 'freelance invoice templates'" is specific. Prioritize free or low-cost channels for early stage.

**Common mistakes:**
- Listing aspirational channels you can't actually execute ("viral growth," "partnerships with enterprise").
- Ignoring the channel-segment fit. Your customers are somewhere specific — go there.
- Forgetting delivery channels. How does the product actually reach them?

**Connects to:** Customer Segments (where they are), Cost Structure (channel costs), Revenue Streams (how they buy).

### 6. Revenue Streams

State how you make money and how much you expect to charge. Be specific about the model.

**Fill with:** Pricing model (subscription, one-time, freemium, transaction fee), price point or range, and who pays. Include a rough calculation: `[number of customers] x [price] = [revenue target]`.

**Common mistakes:**
- "We'll figure out pricing later." Price is part of the product. State a hypothesis now.
- No connection to what the customer values. Price should relate to the problem's cost, not your costs.
- Ignoring willingness to pay. What do they pay for existing alternatives?

**Connects to:** Customer Segments (who pays), Cost Structure (margin = revenue minus cost), Channels (how they purchase).

### 7. Cost Structure

List what it costs to operate this business. Include both building the product and reaching customers.

**Fill with:** Fixed costs (salaries, infrastructure, tools) and variable costs (per-customer costs, channel spend). Identify the biggest cost drivers.

**Common mistakes:**
- Only listing product costs, forgetting acquisition costs.
- Being too precise too early. Rough ranges are fine — the point is identifying cost drivers, not budgeting.
- Missing hidden costs (support, compliance, payment processing).

**Connects to:** Revenue Streams (what margin looks like), Solution (build cost), Channels (acquisition cost).

### 8. Key Metrics

Define how you measure whether this model is working. Pick 3-5 metrics max.

**Fill with:** One metric per stage of the customer lifecycle. Use pirate metrics (AARRR) as a scaffold:

| Stage | Measures | Example |
|-------|----------|---------|
| Acquisition | How they find you | Visitors, signups |
| Activation | First "aha" moment | Completed onboarding, first invoice sent |
| Retention | Do they come back | Weekly active use, month-2 retention |
| Revenue | Do they pay | Conversion rate, ARPU |
| Referral | Do they tell others | NPS, referral rate |

**Common mistakes:**
- Vanity metrics (page views, total signups). Measure what predicts business health.
- Too many metrics. If everything is a key metric, nothing is.
- No leading indicators. Revenue is a lagging indicator — track what predicts it.

**Connects to:** All other boxes. Metrics validate every hypothesis on the canvas.

### 9. Unfair Advantage

Name something you have that can't be easily copied or bought. This is the moat. It's okay to leave this blank early — most startups don't have one yet.

**Fill with:** Insider knowledge, existing audience, network effects, community, proprietary data, personal authority, regulatory advantage, or team expertise. Not features — features can be copied.

**Common mistakes:**
- Listing things that aren't actually hard to copy ("first mover advantage," "great UX," "passion").
- Confusing competitive advantage with unfair advantage. A competitive advantage is temporary; an unfair advantage compounds over time.
- Forcing it. If you don't have one, write "None yet — to be developed" and plan how to build one.

**Connects to:** UVP (amplifies it), Customer Segments (why they stay), Key Metrics (retention and referral).

## When to Use Lean Canvas vs Other Frameworks

| Situation | Use | Why |
|-----------|-----|-----|
| Validating a new business idea | **Lean Canvas** | Maps the full business model in one page |
| Understanding what customers need | **JTBD** | Goes deeper on the job and unmet needs |
| Prioritizing features in a backlog | **JTBD opportunity scoring** | Quantifies importance vs satisfaction |
| Pitching to investors | Lean Canvas as starting point, then expand | Forces you to articulate the core model |
| Evaluating a pivot | **Lean Canvas** side by side | Compare current model vs proposed model |
| Designing a specific feature | **JTBD job stories** | "When [situation], I want to [job], so I can [need]" |
| Competitive analysis | Both | Lean Canvas maps their model; JTBD maps how well they satisfy needs |
| Planning a product launch | Neither — use a launch plan | Lean Canvas is strategy, not execution |

Use lean canvas for breadth (whole business model). Use JTBD for depth (customer needs and jobs). They're complementary.

## Lean Canvas to JTBD Mapping

When both frameworks are relevant, connect them:

| Lean Canvas Box | JTBD Concept | How They Connect |
|-----------------|-------------|------------------|
| **Customer Segments** | Job performer | Same person — described by the job they're hiring a product to do |
| **Problem** | Underserved needs | Problems are clusters of unmet needs within a job |
| **Existing Alternatives** | Current solutions being "fired" | The push force — what's driving switching behavior |
| **UVP** | Why they'd "hire" your product | The pull force — the promise that attracts job performers |
| **Solution** | Product capabilities | Features that satisfy the most underserved needs |
| **Customer Segments (early adopters)** | Most-constrained performers | People whose needs are most underserved by current solutions |
| **Key Metrics (Activation)** | Job completion | The moment the customer successfully gets the job done |
| **Key Metrics (Retention)** | Recurring job frequency | How often the job occurs determines natural usage cadence |
| **Revenue Streams** | Willingness to pay | Correlates with importance of the job and pain of current alternatives |
| **Unfair Advantage** | Switching costs (in your favor) | What creates anxiety about leaving — network effects, data, habit |
| **Channels** | Circumstances / context | Where and when customers encounter the need to get the job done |

## Working With the Canvas

**Fill it in 20 minutes, not 20 days.** Speed matters. The first version is wrong — that's the point. You're making your assumptions explicit so you can test them.

**One canvas per customer segment.** If you have two distinct segments, make two canvases. Merging them hides critical differences.

**Highlight the riskiest box.** After filling it in, circle the box you're least confident about. That's what to test first. Usually it's Problem or Customer Segments, not Solution.

**Revise weekly during early stage.** The canvas is a living document. After each customer conversation, experiment, or data point, update it.

**Read it as a story.** A good canvas tells a coherent narrative: "[Customer] has [problem]. They currently [existing alternative]. We offer [UVP] through [solution], reaching them via [channels]. We charge [revenue model] and it costs us [cost structure]. We'll know it's working when [key metrics]. Over time, [unfair advantage] makes us hard to displace."

**Use it to pressure-test ideas people bring you.** When someone says "we should build X," put it on a canvas. The empty boxes reveal what they haven't thought through.
