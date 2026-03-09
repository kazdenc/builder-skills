# builder-skills

A curated collection of Claude Code skills for product strategy, frontend design, development, and browser automation.

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

### Design

#### Frameworks

Knowledge skills that provide design principles and decision-making context.

| Skill | References | Covers |
|-------|-----------|--------|
| `frontend-design` | 7 | Typography, color, spacing, motion, interaction, responsive, writing |
| `design-foundations` | 5 | Five planes model — strategy, scope, structure, skeleton, surface |

#### Commands

Slash commands for targeted design work. Most accept an optional argument to scope the work (e.g. `/audit header`, `/polish checkout-form`).

| Group | Command | What it does |
|-------|---------|-------------|
| **Setup** | `/design-brief` | One-time setup — gathers design context and saves to config |
| **Review** | `/audit` | Technical quality checks: a11y, performance, theming, responsive |
| | `/critique` | Design review: hierarchy, clarity, emotional resonance |
| **Refine** | `/normalize` | Align with design system standards |
| | `/polish` | Final pass before shipping |
| | `/distill` | Strip to essence, remove complexity |
| | `/clarify` | Improve unclear copy and labels |
| | `/extract` | Pull reusable components into design system |
| **Adjust** | `/bolder` | Amplify safe or boring designs |
| | `/quieter` | Tone down overly bold designs |
| | `/colorize` | Add strategic color |
| | `/animate` | Add purposeful motion |
| | `/delight` | Add moments of joy and personality |
| **Hardening** | `/optimize` | Performance: loading, rendering, bundle size |
| | `/harden` | Error handling, i18n, edge cases |
| | `/adapt` | Adapt for different devices and contexts |
| | `/onboard` | Design onboarding flows and empty states |

### Product

| Skill | References | Covers |
|-------|-----------|--------|
| `jtbd` | 5 | Jobs to Be Done — core concepts, discovering/defining/designing/delivering value |

### Development

| Skill | Covers |
|-------|--------|
| `vercel-react-best-practices` | React/Next.js performance optimization from Vercel Engineering |
| `web-design-guidelines` | Review UI code against Web Interface Guidelines |

### Tools

| Skill | Covers |
|-------|--------|
| `agent-browser` | Browser automation CLI — navigate, fill forms, screenshot, scrape, test |

## Structure

```
.claude/skills/
  design/
    frameworks/
      design-brief/              # one-time setup
      frontend-design/           # 7 reference files
      design-foundations/        # 5 reference files
    review/
      audit/
      critique/
    refine/
      normalize/
      polish/
      distill/
      clarify/
      extract/
    adjust/
      bolder/
      quieter/
      colorize/
      animate/
      delight/
    hardening/
      optimize/
      harden/
      adapt/
      onboard/
  product/
    jtbd/                        # 5 reference files
  dev/
    vercel-react-best-practices/
    web-design-guidelines/
  tools/
    agent-browser/
```

Every skill follows the same structure: `SKILL.md` + optional `references/` directory. See [SKILL-GUIDE.md](SKILL-GUIDE.md) for writing conventions.

## Credits

JTBD skills based on *The Jobs to Be Done Playbook* by Jim Kalbach (Rosenfeld Media). Design skills adapted from [pbakaus/impeccable](https://github.com/pbakaus/impeccable), building on [Anthropic's frontend-design skill](https://github.com/anthropics/skills/tree/main/skills/frontend-design). Vercel skills from [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills). Browser automation from [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser).
