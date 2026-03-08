# Surface Plane

The top of the model — what users actually see, hear, and touch. Surface is where all the decisions from the planes below become a sensory experience. It's the most visible layer, but it only works when it's built on solid foundations.

## Sensory Design

Surface design is primarily visual for most digital products, but consider all senses:

- **Vision**: Layout, color, typography, imagery, motion — the dominant channel
- **Touch**: Haptic feedback, physical controls, tactile materials (hardware products)
- **Hearing**: Sound effects, audio feedback, voice interfaces
- **Smell/Taste**: Rarely relevant in digital, but crucial in physical product design

## Follow the Eye

Users don't read pages — they scan them. Design for how eyes actually move:

### Eye Tracking Patterns

- **F-pattern**: Users scan horizontally across the top, then down the left side, with shorter horizontal scans. Common on text-heavy pages.
- **Z-pattern**: Eyes move top-left → top-right → bottom-left → bottom-right. Common on less text-heavy pages and landing pages.
- **Focal point**: A dominant visual element captures attention first, then eyes flow to secondary elements.

### Directing Attention

Design should guide the eye in the order that serves the user's goals:

1. **Size**: Larger elements attract first
2. **Color/Contrast**: High contrast draws the eye
3. **Position**: Top-left gets seen first (in LTR languages)
4. **Whitespace**: Isolation draws attention — an element surrounded by space stands out
5. **Motion**: Movement captures attention instantly (use sparingly — it's powerful and distracting)

**Ask**: If a user glances at this screen for 2 seconds, will they see the most important element first? If not, the visual hierarchy is wrong.

## Contrast and Uniformity

These are the two fundamental tools of surface design. They work in tension:

### Contrast

Contrast draws attention to differences. Use it to:

- **Establish hierarchy**: Headings vs body, primary vs secondary actions
- **Signal interactivity**: Clickable elements should look different from static ones
- **Group by difference**: Different sections should be visually distinct

Contrast operates across multiple dimensions simultaneously:
- Size (large vs small)
- Color (saturated vs muted, light vs dark)
- Weight (bold vs light)
- Shape (rounded vs angular)
- Density (packed vs sparse)

### Uniformity

Uniformity signals that things belong together. Use it to:

- **Group related elements**: Same visual treatment = same category
- **Create patterns**: Consistent styling lets users predict behavior
- **Build rhythm**: Repeated spacing, sizing, and alignment create visual flow

**The tension**: Too much contrast = chaos. Too much uniformity = monotony. Effective surface design balances both — enough uniformity to create coherence, enough contrast to create hierarchy.

## Consistency

### Internal Consistency

The product should be consistent with itself:

- **Same elements look the same.** A button should look like a button everywhere. A heading at one size/weight in one section shouldn't change in another.
- **Same patterns behave the same.** If clicking an icon in one place opens a panel, clicking a similar icon elsewhere should behave similarly.
- **Design tokens enforce consistency.** Colors, spacing, typography defined as reusable values — not ad hoc choices.

### External Consistency

The product should be consistent with user expectations from the broader ecosystem:

- Platform conventions (iOS patterns on iOS, Android patterns on Android)
- Industry conventions (e-commerce patterns, SaaS patterns)
- Web conventions (underlined text = link, X = close)

**Breaking consistency** requires strong justification. Every departure from convention adds cognitive load.

## Color

Color communicates before users read a single word.

### Color Functions

- **Brand identity**: Dominant colors signal who this is
- **Hierarchy**: Color draws attention to what matters
- **Semantics**: Red = error/danger, green = success, yellow = warning (culturally dependent)
- **Grouping**: Consistent color coding categorizes information
- **Emotion**: Warm colors feel energetic, cool colors feel calm

### Color Principles

- **Limit the palette.** 1-2 primary colors, 1-2 accents, plus neutrals. More creates noise.
- **Ensure contrast.** WCAG AA minimum: 4.5:1 for text, 3:1 for UI elements.
- **Don't rely on color alone.** Always pair with icons, labels, or patterns for accessibility.
- **Tint your neutrals.** Pure gray is lifeless. Add subtle warmth or coolness.
- **Test for color blindness.** ~8% of men have some form. Red/green combinations are the most common issue.

## Typography

Type is the primary vehicle for information in most interfaces.

### Type Hierarchy

Create clear levels that signal importance:

- **Display**: Page titles, hero text — largest, most distinctive
- **Heading levels**: Section structure — progressively smaller
- **Body**: Primary reading text — optimized for readability
- **Caption/Label**: Supporting text — smaller, often lighter weight

### Readability

- **Line length**: 45-75 characters for body text
- **Line height**: 1.4-1.6x font size for body text
- **Font size**: 16px minimum for body text on screens
- **Contrast**: Dark text on light background is easier to read for extended content

### Font Selection

- Choose fonts that match the product's tone and brand
- Limit to 2 fonts maximum (one display, one body)
- Ensure the font has all needed weights and styles
- Test with real content, not just specimen text

## Design Comps and Style Guides

### Design Comps

Full visual mockups showing final appearance. Should:

- Cover all key pages/states, not just the happy path
- Show responsive behavior at key breakpoints
- Include edge cases (long text, empty states, errors)
- Be annotated with interaction notes

### Style Guides

Codify visual decisions for consistent implementation:

- Color palette with usage guidelines
- Typography scale with usage context
- Spacing system
- Component library with states and variants
- Iconography style and usage rules
- Voice and tone guidelines

A style guide is only valuable if it's maintained and used. A beautiful guide that developers ignore is decorative, not functional.

## Common Surface Failures

- **Visual hierarchy doesn't match content hierarchy** — Pretty but users can't find what matters
- **Inconsistency** — Same elements styled differently across pages
- **Decoration over communication** — Visual effects that don't serve comprehension
- **Ignoring the planes below** — Beautiful surface on broken structure/scope/strategy
- **Color as sole differentiator** — Fails for colorblind users and in grayscale contexts
- **Typography that sacrifices readability** — Display fonts used for body text, insufficient contrast
- **No design system** — Every page is a one-off, maintenance becomes impossible
