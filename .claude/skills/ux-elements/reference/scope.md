# Scope Plane

Strategy asks "why?" — Scope asks "what?" Translate product objectives and user needs into specific requirements for what the product will (and won't) offer.

## Why Define Scope

Two reasons, equally important:

1. **So you know what you're building.** Without defined scope, everyone has a slightly different product in their head. Requirements give the team a shared reference and common language.

2. **So you know what you're NOT building.** Every good idea doesn't belong in this release. Defined scope gives you a framework to evaluate new ideas and defer what doesn't fit.

Without scope, you get scope creep — each small addition seems harmless, but together they create an unshippable product with no clear finish line.

## The Duality on This Plane

| Functionality Side | Information Side |
|-------------------|-----------------|
| Functional specifications | Content requirements |
| What the system does | What information it provides |
| Features, behaviors, rules | Text, images, audio, video, data |

These aren't separate — they inform each other. Every feature has content implications (error messages, labels, instructions). Every content decision has functional implications (CMS needs, delivery mechanisms).

## Defining Requirements

Requirements come from three sources:

1. **What people say they want** — The obvious requests. Some are good, some mask deeper needs.
2. **What people actually need** — Discovered by exploring *why* they want something. The stated request may address a symptom, not the cause.
3. **What people don't know they need** — Emerges from brainstorming, cross-functional discussion, and observing gaps in existing solutions.

### Writing Good Requirements

**Be positive.** Describe what the system *will* do, not what it won't.
- Bad: "The system will not allow purchase without kite string"
- Good: "The system will direct users to the kite string page if they try to buy a kite without string"

**Be specific.** Remove ambiguity so you can tell whether a requirement is met.
- Bad: "The most popular videos will be highlighted"
- Good: "Videos with the most views in the last week will appear at the top of the list"

**Avoid subjective language.** Requirements must be falsifiable.
- Bad: "The site will have a hip, flashy style"
- Good: "The visual design will conform to the brand guidelines document"

**Make it quantitative when possible.**
- Bad: "The system should have high performance"
- Good: "The system will support at least 1,000 simultaneous users"

## Functional Specifications

Specs don't need to be exhaustive — they need to be clear. Focus on what needs definition to prevent confusion during design and development.

**Common failure**: Treating specs as a one-time document that's filed away. Specs should be lightweight and updated as decisions change during implementation. A spec that doesn't reflect reality is worse than no spec.

## Content Requirements

For every content feature, define:

- **Content types**: Text, images, audio, video, data
- **Approximate size**: Word counts, image dimensions, file sizes
- **Owner**: Who creates and maintains this content
- **Update frequency**: How often it needs refreshing, based on user expectations and organizational capacity
- **Target audience**: Which user segment this serves (affects tone, complexity, format)

**Content is hard work.** Creating content for launch is easy compared to maintaining it. Every content requirement needs a realistic maintenance plan.

**Don't confuse format with purpose.** "We need FAQs" describes a format. The purpose is "provide ready access to commonly needed information" — which might be better served by contextual help, a search system, or inline guidance.

## Prioritizing Requirements

Evaluate every requirement against three criteria:

1. **Does it fulfill a strategic objective?** (product objectives or user needs)
2. **Is it feasible?** (technically possible, within resource and time constraints)
3. **What are the dependencies?** (does it conflict with or depend on other requirements?)

### Handling Conflicts

- One requirement may serve multiple objectives — high priority
- Stakeholders often confuse features with strategy — redirect to goals
- When priorities are unclear, appeal to the defined strategy, not politics
- Requirements that don't fit now can be stockpiled for future releases

## Common Scope Failures

- **No defined scope** — Perpetual beta, features in various states of completeness
- **Scope creep** — Each small addition seems harmless until the snowball is out of control
- **Requirements by committee** — Everyone's pet feature gets in, coherence is lost
- **Content as afterthought** — "Somebody will write the error messages later" — they won't
- **Mistaking format for purpose** — Building FAQs when users need contextual help
