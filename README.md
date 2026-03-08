# builder-skills

A curated collection of Claude Code skills for frontend design, development, and browser automation.

## Setup

Copy `.claude/skills/` into any project:

```bash
cp -r .claude/skills/ your-project/.claude/skills/
```

Or install globally so skills are available everywhere:

```bash
cp -r .claude/skills/* ~/.claude/skills/
```

## Skills

### Design System

The core `frontend-design` skill is auto-loaded as context whenever design commands run. It includes 7 reference files:

| Reference | Covers |
|-----------|--------|
| typography | Type systems, font pairing, modular scales, OpenType |
| color-and-contrast | OKLCH, tinted neutrals, dark mode, accessibility |
| spatial-design | Spacing systems, grids, visual hierarchy |
| motion-design | Easing curves, staggering, reduced motion |
| interaction-design | Forms, focus states, loading patterns |
| responsive-design | Mobile-first, fluid design, container queries |
| ux-writing | Button labels, error messages, empty states |

### Design Commands

Slash commands for targeted design work. Most accept an optional argument to scope the work (e.g. `/audit header`, `/polish checkout-form`).

| Command | What it does |
|---------|-------------|
| `/teach-design` | One-time setup — gathers design context and saves to config |
| **Review** | |
| `/audit` | Technical quality checks: a11y, performance, theming, responsive |
| `/critique` | UX design review: hierarchy, clarity, emotional resonance |
| **Refine** | |
| `/normalize` | Align with design system standards |
| `/polish` | Final pass before shipping |
| `/distill` | Strip to essence, remove complexity |
| `/clarify` | Improve unclear UX copy and labels |
| `/extract` | Pull reusable components into design system |
| **Adjust** | |
| `/bolder` | Amplify safe or boring designs |
| `/quieter` | Tone down overly bold designs |
| `/colorize` | Add strategic color |
| `/animate` | Add purposeful motion |
| `/delight` | Add moments of joy and personality |
| **Harden** | |
| `/optimize` | Performance: loading, rendering, bundle size |
| `/harden` | Error handling, i18n, edge cases |
| `/adapt` | Adapt for different devices and contexts |
| `/onboard` | Design onboarding flows and empty states |

### Design Foundations

The `design-foundations` skill provides a structured framework for thinking about product design across five planes, from abstract strategy to concrete surface design. Includes 5 reference files:

| Reference | Covers |
|-----------|--------|
| strategy | Product objectives, user needs, personas, success metrics |
| scope | Functional specs, content requirements, prioritization |
| structure | Interaction design, information architecture, conceptual models |
| skeleton | Interface design, navigation design, wireframes |
| surface | Sensory design, color, typography, contrast, consistency |

### Development

| Skill | Description |
|-------|-------------|
| `vercel-react-best-practices` | React/Next.js performance optimization from Vercel Engineering |
| `web-design-guidelines` | Review UI code against Web Interface Guidelines |

### Browser Automation

| Skill | Description |
|-------|-------------|
| `agent-browser` | Browser automation CLI — navigate, fill forms, screenshot, scrape, test |

## Structure

```
.claude/skills/
  frontend-design/          # Core design system
    SKILL.md
    reference/
      typography.md
      color-and-contrast.md
      spatial-design.md
      motion-design.md
      interaction-design.md
      responsive-design.md
      ux-writing.md
  teach-design/             # Setup
  audit/                    # Review
  critique/
  normalize/                # Refine
  polish/
  distill/
  clarify/
  extract/
  bolder/                   # Adjust
  quieter/
  colorize/
  animate/
  delight/
  optimize/                 # Harden
  harden/
  adapt/
  onboard/
  design-foundations/          # Five planes framework
    SKILL.md
    reference/
      strategy.md
      scope.md
      structure.md
      skeleton.md
      surface.md
  vercel-react-best-practices/  # Development
  web-design-guidelines/
  agent-browser/                # Tools
```

## Credits

Design skills adapted from [pbakaus/impeccable](https://github.com/pbakaus/impeccable), building on [Anthropic's frontend-design skill](https://github.com/anthropics/skills/tree/main/skills/frontend-design). Vercel skills from [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills). Browser automation from [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser).
