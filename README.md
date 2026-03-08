# builder-skills

A curated collection of Claude Code skills for frontend design, development, and browser automation.

## Setup

Copy or symlink `.claude/skills/` into any project to make these skills available in Claude Code:

```bash
# Copy into a project
cp -r .claude/skills/ your-project/.claude/skills/

# Or symlink for shared use
ln -s /path/to/builder-skills/.claude/skills /path/to/your-project/.claude/skills
```

## Skills

### Core Design Skill

| Skill | Description |
|-------|-------------|
| `frontend-design` | Comprehensive design system with 7 reference files covering typography, color, spatial design, motion, interaction, responsive design, and UX writing. Auto-loaded as context for other design skills. |

### Design Commands

Slash commands for targeted design work. Most accept an optional argument to focus on a specific area (e.g. `/audit header`).

| Command | Description |
|---------|-------------|
| `/teach-design` | One-time setup — gathers design context for your project and saves it to config |
| `/audit` | Technical quality checks: accessibility, performance, theming, responsive |
| `/critique` | UX design review: hierarchy, clarity, emotional resonance |
| `/normalize` | Align a feature with your design system standards |
| `/polish` | Final pass before shipping — alignment, spacing, consistency |
| `/distill` | Strip to essence, remove unnecessary complexity |
| `/clarify` | Improve unclear UX copy, error messages, labels |
| `/optimize` | Performance improvements: loading, rendering, bundle size |
| `/harden` | Error handling, i18n, edge cases, production resilience |
| `/animate` | Add purposeful motion and micro-interactions |
| `/colorize` | Introduce strategic color to monochromatic designs |
| `/bolder` | Amplify safe or boring designs |
| `/quieter` | Tone down overly bold or aggressive designs |
| `/delight` | Add moments of joy and personality |
| `/extract` | Pull reusable components into your design system |
| `/adapt` | Adapt designs for different devices and contexts |
| `/onboard` | Design onboarding flows and empty states |

### Development

| Skill | Description |
|-------|-------------|
| `vercel-react-best-practices` | React and Next.js performance optimization guidelines from Vercel Engineering |
| `web-design-guidelines` | Review UI code against Web Interface Guidelines for accessibility and UX best practices |

### Browser Automation

| Skill | Description |
|-------|-------------|
| `agent-browser` | Full browser automation CLI — navigate, fill forms, click, screenshot, scrape data, test web apps |

## Structure

```
.claude/
  skills/
    frontend-design/       # Core design skill + 7 reference files
      SKILL.md
      reference/
        typography.md
        color-and-contrast.md
        spatial-design.md
        motion-design.md
        interaction-design.md
        responsive-design.md
        ux-writing.md
    audit/SKILL.md          # Design command skills
    critique/SKILL.md
    normalize/SKILL.md
    polish/SKILL.md
    distill/SKILL.md
    clarify/SKILL.md
    optimize/SKILL.md
    harden/SKILL.md
    animate/SKILL.md
    colorize/SKILL.md
    bolder/SKILL.md
    quieter/SKILL.md
    delight/SKILL.md
    extract/SKILL.md
    adapt/SKILL.md
    onboard/SKILL.md
    teach-design/SKILL.md
    vercel-react-best-practices/SKILL.md
    web-design-guidelines/SKILL.md
    agent-browser/SKILL.md
```

## Credits

Design skills adapted from [pbakaus/impeccable](https://github.com/pbakaus/impeccable), which builds on [Anthropic's frontend-design skill](https://github.com/anthropics/skills/tree/main/skills/frontend-design). Vercel skills from [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills). Browser automation from [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser).
