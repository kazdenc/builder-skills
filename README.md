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

#### Frameworks

Knowledge skills that provide product strategy and decision-making context.

| Skill | References | Covers |
|-------|-----------|--------|
| `jtbd` | 5 | Jobs to Be Done — core concepts, discovering/defining/designing/delivering value |
| `lean-canvas` | — | One-page business model — problem, solution, channels, metrics, unfair advantage |

#### Commands

Slash commands for product management work. Most accept an optional argument to scope the work (e.g. `/prd search-feature`, `/prioritize backlog`).

| Group | Command | What it does |
|-------|---------|-------------|
| **Discover** | `/interview` | Generate interview scripts or analyze transcripts for insights |
| | `/competitive` | Competitive analysis with feature comparison and positioning gaps |
| **Define** | `/prd` | Write a product requirements document |
| | `/stories` | Write job stories with acceptance criteria |
| | `/spec` | Write a technical specification from requirements |
| **Prioritize** | `/prioritize` | Score and rank features using RICE, opportunity scoring, or impact/effort |
| | `/scope` | Cut scope to identify MVP and must-haves for a timeline |
| **Launch** | `/gtm` | Go-to-market plan with positioning, messaging, and launch checklist |
| | `/metrics` | Define success metrics, KPIs, and instrumentation plan |
| | `/retro` | Run a structured retrospective with actionable outcomes |

### Development

#### Frameworks

Knowledge skills that provide development best practices and conventions.

| Skill | Covers |
|-------|--------|
| `vercel-react-best-practices` | React/Next.js performance optimization from Vercel Engineering (58 rules) |
| `web-design-guidelines` | Review UI code against Web Interface Guidelines |
| `typescript-patterns` | Type design, generics, utility types, discriminated unions, error handling |
| `api-design` | RESTful conventions, error formats, pagination, versioning, auth patterns |

#### Commands

Slash commands for development workflows. Most accept an optional argument to scope the work.

| Group | Command | What it does |
|-------|---------|-------------|
| **Setup** | `/nextjs-init` | Scaffold Next.js with App Router, TypeScript, Tailwind, opinionated defaults |
| | `/supabase-setup` | Initialize Supabase: schema, RLS policies, auth, storage, edge functions |
| | `/vercel-deploy` | Deploy to Vercel: config, env vars, domains, preview deploys |
| | `/db-schema` | Design a database schema from requirements (Postgres-first, Supabase-aware) |
| | `/env-config` | Set up environment variables across local/staging/production with validation |
| **Review** | `/code-review` | Structured code review: correctness, readability, performance, security |
| | `/security-scan` | OWASP top 10 check: injection, XSS, auth issues, secrets exposure |
| | `/deps-audit` | Audit dependencies: outdated, vulnerable, unused, bloated |
| **Test** | `/test-plan` | Write test strategy with coverage targets and testing pyramid |
| | `/test-write` | Generate tests for existing code (unit, integration, e2e) |
| **Deploy** | `/deploy-check` | Pre-deployment checklist: migrations, env vars, rollback plan, smoke tests |
| | `/monitor` | Set up monitoring: error tracking, alerts, dashboards, SLOs |

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
    frameworks/
      jtbd/                      # 5 reference files
      lean-canvas/
    discover/
      interview/
      competitive/
    define/
      prd/
      stories/
      spec/
    prioritize/
      prioritize/
      scope/
    launch/
      gtm/
      metrics/
      retro/
  dev/
    frameworks/
      vercel-react-best-practices/
      web-design-guidelines/
      typescript-patterns/
      api-design/
    setup/
      nextjs-init/
      supabase-setup/
      vercel-deploy/
      db-schema/
      env-config/
    review/
      code-review/
      security-scan/
      deps-audit/
    test/
      test-plan/
      test-write/
    deploy/
      deploy-check/
      monitor/
  tools/
    agent-browser/
```

Every skill follows the same structure: `SKILL.md` + optional `references/` directory. See [SKILL-GUIDE.md](SKILL-GUIDE.md) for writing conventions.

## Credits

JTBD skills based on *The Jobs to Be Done Playbook* by Jim Kalbach (Rosenfeld Media). Design skills adapted from [pbakaus/impeccable](https://github.com/pbakaus/impeccable), building on [Anthropic's frontend-design skill](https://github.com/anthropics/skills/tree/main/skills/frontend-design). Vercel skills from [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills). Browser automation from [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser).
