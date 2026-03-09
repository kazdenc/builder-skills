# Structure Plane

Scope defines what you're building. Structure defines how the pieces fit together — how users move through the product and how information is organized.

## The Duality on This Plane

| Functionality Side | Information Side |
|-------------------|-----------------|
| Interaction design | Information architecture |
| How the system behaves in response to user actions | How information is organized so users can find and understand it |

Both share a core concern: defining patterns and sequences in which options are presented to users.

## Interaction Design

Interaction design describes the dance between user and system. The user acts, the system responds, the user reacts to the response — and so it goes. Good interaction design anticipates the user's moves rather than forcing them to adapt.

### Conceptual Models

Users bring mental models of how things work. A shopping cart is a container — you put things in, take things out. An order form is a document — you fill it out and submit it. The model you choose shapes the entire interaction.

**Guidelines**:
- Choose models users already understand — familiar models reduce learning
- Be consistent — if it's a container, it's always a container, not sometimes a list
- Don't take metaphors too literally — a phone icon for making reservations is too literal
- Convention is powerful — the shopping cart pattern is so established that breaking it needs strong justification

### Error Handling

Design errors out in layers:

1. **Make errors impossible.** Disable invalid options. Use constrained inputs (date pickers instead of text fields). Don't let users reach states that shouldn't exist.

2. **Make errors difficult.** Require confirmation for destructive actions. Use clear labels that prevent misunderstanding.

3. **Help users catch errors.** Inline validation, clear error messages, visual indicators. Show what's wrong and where.

4. **Help users recover.** Undo, back, edit. Let users fix mistakes without starting over.

5. **Prevent data loss.** Auto-save. Warn before navigating away from unsaved work.

**Watch out**: Too many "Are you sure?" confirmations train users to click through them blindly, defeating the purpose. Reserve confirmation for truly destructive, irreversible actions.

### Flow Design

Map out how users move through tasks:

- What's the entry point? Where do users come from?
- What's the critical path? The minimum steps to complete the task.
- What are the branch points? Where do users make choices?
- What are the error paths? Where do things go wrong and how do users get back on track?
- What's the exit? How do users know they're done?

**Keep flows shallow.** Every additional step is a point where users can drop off. If a task requires many steps, break it into clearly labeled stages with progress indication.

## Information Architecture

How information is organized, grouped, and labeled so users can find what they need and understand what they find.

### Organizational Approaches

**Top-down**: Start with broad categories, subdivide into specifics. Good when you know the full scope of content and users have clear goals. Risk: categories may not match how users think.

**Bottom-up**: Start with individual content items, group by similarity. Good for large, diverse content sets. Risk: can produce sprawling, incoherent structures.

**Both**: Most real projects use a hybrid — top-down for high-level structure, bottom-up to validate groupings match actual content.

### Structuring Principles

- **Organize by user mental models**, not organizational structure. Users don't care about your org chart.
- **Use mutually exclusive categories** when possible. If an item could go in multiple places, your categories overlap.
- **Keep hierarchy depth manageable.** 3-4 levels maximum. Broad and shallow beats narrow and deep.
- **Label clearly.** Category names should be unambiguous and use language users understand, not internal jargon.

### Architectural Patterns

| Pattern | When to Use |
|---------|------------|
| **Hierarchical (tree)** | Content with clear parent-child relationships. Most common pattern. |
| **Matrix** | Users need to navigate the same content along multiple dimensions (e.g., by topic AND by date). |
| **Sequential** | Content that must be consumed in order (tutorials, onboarding, checkout). |
| **Organic/hub-and-spoke** | Exploratory experiences without strict hierarchy (wikis, related content). |

Most products combine patterns — a hierarchical main structure with sequential flows for specific tasks and organic cross-linking between related content.

### Metadata and Labeling

Metadata enables flexible organization and findability:

- **Descriptive metadata**: What is this? (title, author, date, type)
- **Administrative metadata**: How is this managed? (owner, status, update frequency)
- **Structural metadata**: How does this relate to other content? (tags, categories, relationships)

**Controlled vocabularies** — agreed-upon lists of terms for labeling content — prevent the chaos of everyone using different words for the same thing. "FAQ" vs "Help" vs "Support" vs "Questions" — pick one and be consistent.

## Common Structure Failures

- **Org chart as IA** — Departments as top-level navigation when users don't think in departments
- **Too deep** — Burying content 5+ levels deep where no one will find it
- **Inconsistent models** — Treating the same thing as a place, then an object, then a list depending on context
- **No error recovery** — Dead ends where users can't get back or undo
- **Designer's model vs user's model** — Organizing by what makes sense to the team, not to users
