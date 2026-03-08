# Skeleton Plane

Structure defines the conceptual organization. Skeleton makes it concrete — the specific arrangement of interface elements, navigation mechanisms, and information presentation that users actually see and interact with.

## The Duality on This Plane

| Functionality Side | Information Side |
|-------------------|-----------------|
| Interface design | Navigation design + Information design |
| Arranging elements to enable interaction | Presenting information so users can find and understand it |

Information design bridges both sides — the presentation of data supports both task completion and content comprehension.

## Interface Design

Arrange interface elements so users can interact with the system's functionality. The goal: make the most important things the most visible, and make everything usable without instruction.

### Core Principles

**Make the default path obvious.** Users should immediately see the primary action. If they have to search for what to do next, the interface has failed.

**Reduce choices at each point.** Don't present every option simultaneously. Group related controls. Use progressive disclosure — show basics first, details on demand.

**Leverage convention.** Users bring expectations from every other product they've used. Logos go top-left. Navigation goes top or left. Primary actions go bottom-right in dialogs. Breaking convention requires strong justification.

**Use metaphor carefully.** Interface metaphors (desktop, folders, trash) help users transfer existing knowledge. But don't stretch metaphors beyond their usefulness — digital products can do things physical objects can't.

### Interface Patterns

- **Selection**: Present options users choose from (dropdowns, radio buttons, toggles). Use when the set of valid options is known and finite.
- **Input**: Let users provide data (text fields, date pickers, file uploads). Constrain inputs to valid formats when possible.
- **Navigation**: Move users between content and functions (links, tabs, breadcrumbs). See Navigation Design below.
- **Display**: Present information (tables, lists, cards, charts). Match the display pattern to what users need to do with the information.
- **Feedback**: Show system status (progress bars, success/error states, loading indicators). Users should never wonder "did that work?"

## Navigation Design

Navigation serves three simultaneous purposes:

1. **Access**: A way to get from one place to another
2. **Orientation**: Tell users where they are now
3. **Understanding**: Show users what this product contains

### Navigation Systems

**Global navigation**: Persists across all pages/views. Provides access to top-level sections and orientation within the product. Should be consistent in position and behavior.

**Local navigation**: Specific to the current section. Shows what's available within this area. Changes based on context.

**Supplementary navigation**: Provides shortcuts to related content that doesn't fit the main hierarchy (related articles, recently viewed, frequently used).

**Contextual navigation**: Embedded in content — links within text, "see also" references, related items. Enables organic exploration.

**Breadcrumbs**: Show the user's path through the hierarchy. Most useful in deep, hierarchical structures. Not a substitute for good navigation — a supplement.

### Navigation Principles

- **Be consistent.** Navigation that changes position, appearance, or behavior across pages destroys confidence.
- **Show current location.** Highlight the active section. Users who can't tell where they are will leave.
- **Don't depend on browser controls.** Back buttons are unreliable. Provide your own navigation.
- **Minimize clicks to content.** Every click is a decision point and a potential drop-off.
- **Label clearly.** Navigation labels should describe destinations, not actions. Use language users understand.

## Information Design

Present information so users can understand it. This applies to everything from data tables to error messages to form labels.

### Principles

**Group related information.** Proximity signals relationship. Elements that belong together should be visually grouped.

**Create clear hierarchy.** Users scan before they read. Size, weight, color, and position should communicate what's most important.

**Support scanning.** Most users don't read — they scan. Use headings, bullet points, bold key terms, short paragraphs. Front-load important information.

**Use appropriate representations.** Numbers in tables. Trends in line charts. Proportions in pie charts. Geography in maps. Match the representation to what users need to understand.

**Reduce noise.** Every element on screen competes for attention. Remove anything that doesn't serve the user's immediate goal.

## Wireframes

Wireframes bring together interface design, navigation design, and information design into a single document that shows the arrangement of elements on a page.

### What Wireframes Show

- Element placement and relative sizing
- Content hierarchy and grouping
- Navigation placement and structure
- Functional elements and their arrangement
- Relationships between elements

### What Wireframes Don't Show

- Visual design (color, typography, imagery)
- Final content (use realistic placeholders, not lorem ipsum)
- Interactive behavior (use annotations or separate flow documents)

### Wireframe Principles

- **Annotate.** A wireframe without annotations is ambiguous. Explain behavior, constraints, and edge cases.
- **Use realistic content.** Real headlines, real data lengths, real labels. "Lorem ipsum" hides content problems.
- **Show states.** Empty, loading, error, populated. A wireframe that only shows the happy path is incomplete.
- **Keep fidelity appropriate.** Low-fi for early exploration, higher-fi as decisions solidify. Don't polish wireframes — they should look disposable.

## Common Skeleton Failures

- **No clear primary action** — Everything has equal visual weight, nothing stands out
- **Navigation doesn't communicate structure** — Users can move around but can't build a mental model of the product
- **Information overload** — Everything visible at once with no hierarchy or grouping
- **Inconsistent layout** — Elements jump around between pages, users can't predict where things are
- **Breaking convention without reason** — Novel interfaces that confuse rather than improve
- **Wireframes without annotations** — Developers interpret ambiguity differently than designers intended
