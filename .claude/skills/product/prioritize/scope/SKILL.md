---
name: scope
description: Cut scope ruthlessly to identify MVP, must-haves, nice-to-haves, and out-of-scope items for a given timeline. Use when user says "cut scope", "what's the MVP", "scope this down", "we have X weeks", "reduce scope", "what can we cut", or needs to ship something meaningful within constraints.
user-invokable: true
args:
  - name: target
    description: The feature set, PRD, or project to scope (optional)
    required: false
  - name: timeline
    description: The time constraint (e.g., "2 weeks", "Q2") (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

Cut scope to ship something meaningful within constraints. Favor a complete small thing over an incomplete big thing.

## Step 1: List All Requirements

Collect every feature, requirement, and "wouldn't it be nice if" into a flat list. If the user provided a PRD or feature set, extract items from it. If not, ask.

Each item should be a concrete, shippable unit — not a theme. Break down anything vague ("improve onboarding") into specific deliverables ("add welcome wizard", "send day-1 email", "add progress indicator").

## Step 2: Apply the Core Job Test

For each item, ask: **"If we shipped without this, would the core job still get done?"**

- **Yes** → Nice-to-have. Move it down.
- **No** → Must-have candidate. Keep it.

Be honest. "Get done" means the user can complete the primary task, even if it's manual, ugly, or slow. A to-do app without drag-and-drop reordering still lets people track tasks. A to-do app without the ability to add tasks does not.

## Step 3: Categorize into Tiers

Sort every item into one of four tiers:

| Tier | Label | Criteria | Ships in |
|------|-------|----------|----------|
| **T0** | Must Ship (MVP) | Without this, the product doesn't work or the core job can't be completed | Current timeline |
| **T1** | Should Ship | Makes the experience significantly better but isn't blocking the core job | v1.1 / next cycle |
| **T2** | Could Ship | Genuine value, but the product is viable and useful without it | Future |
| **T3** | Won't Ship | Out of scope, wrong timing, low value, or conflicts with focus | Never (for now) |

Present the result as a table:

```markdown
| Tier | Item | Rationale |
|------|------|-----------|
| T0   | ...  | Core job requires it: [why] |
| T1   | ...  | Improves [what], but core flow works without it |
| T2   | ...  | Nice for [who], revisit after launch |
| T3   | ...  | Out of scope because [reason] |
```

## Step 4: Validate the MVP

Check that the T0 set is coherent:

- **Does it complete one whole job?** Users should be able to go from start to done, even if the path is rough. Partial flows are worse than missing features.
- **Is it scoped to one persona?** Serving one user well beats serving three users poorly. If T0 tries to cover multiple personas, pick the most important one and move the rest to T1.
- **Can it ship within the timeline?** If T0 alone exceeds the timeline, cut further. Ask: "Which of these must-haves is the *most* must-have?"
- **Is there a simpler version of any T0 item?** "Full search with filters" might become "basic text search." "Role-based permissions" might become "admin/member only." Reduce the scope *within* must-haves, not just between them.

## Scoping Principles

Follow these when making cuts:

| Principle | Instead of... | Do this |
|-----------|--------------|---------|
| **Depth over breadth** | 5 features at 60% quality | 2 features at 100% quality |
| **Complete flows over partial features** | Login + half of onboarding + half of dashboard | Login + complete onboarding |
| **One persona done well** | Basic support for 3 user types | Full support for 1 user type |
| **Manual before automated** | Build the automation | Do it manually, learn, then automate |
| **Ugly but functional** | Polished UI for an unproven flow | Working flow with minimal styling |
| **Read before write** | Full CRUD from day one | Display data first, add editing later |

## Anti-Patterns: Scope Creep Signals

Watch for these and call them out:

- **"Just one more thing"** — Every addition resets the timeline. Ask: "What are we cutting to make room for this?"
- **"It's only a small change"** — Small changes have invisible costs: edge cases, testing, documentation, future maintenance.
- **"Users will expect it"** — Maybe. But will they refuse to use the product without it? If not, it's T1.
- **"While we're at it"** — Adjacent work feels efficient but fragments focus. Ship the current scope first.
- **"We need parity with [competitor]"** — You don't. You need to do one job better than they do, not every job equally.
- **"It won't take long"** — It will. Multiply the estimate by 2 and ask if it's still worth it.

## Output Format

Deliver:
1. The tiered table with every item categorized and justified
2. A clear MVP statement: "The MVP lets [persona] do [core job] by shipping [T0 items]"
3. Timeline fit: Does T0 fit the constraint? If not, what further cuts are needed?
4. Scope creep flags: Any items that tried to sneak into T0 but belong in T1+
