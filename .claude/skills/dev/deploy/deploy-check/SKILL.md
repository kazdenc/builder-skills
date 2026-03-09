---
name: deploy-check
description: Pre-deployment checklist covering migrations, environment variables, feature flags, rollback plan, and smoke tests. Use when user says "ready to deploy", "deployment checklist", "pre-deploy check", "can I ship this", "deploy review", or needs to verify everything is ready before pushing to production.
user-invokable: true
args:
  - name: target
    description: The feature or release to check (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

# Pre-Deployment Check

Walk through this checklist with the user before any production deploy. If a `target` is provided, scope the check to that feature or release. Present each category, confirm status, and flag anything unresolved.

## Pre-Deploy Checklist

Work through each category. Mark items as pass, fail, or N/A.

### Code

| Check | Details |
|-------|---------|
| All tests pass | Run the full test suite. No skipped tests unless documented. |
| No console.logs / debuggers | Search for `console.log`, `console.debug`, `debugger` statements in changed files. |
| Linting clean | `lint` command exits with zero errors and zero warnings. |
| PR approved | At least one approval from a reviewer who understands the changed area. |
| Changes rebased | Branch is up to date with the target branch. No merge conflicts. |

### Database

| Check | Details |
|-------|---------|
| Migrations tested locally | Run migrations on a fresh local database and verify the schema. |
| Migrations reversible | Every `up` has a corresponding `down`. Test the rollback. |
| No breaking schema changes | Dropping columns, renaming tables, or changing types requires a multi-step migration plan. |
| Seeds updated | If migrations change structure, seed files still work. |

### Environment

| Check | Details |
|-------|---------|
| New env vars in production | Every new variable from `.env.local` is set in the production environment. |
| Secrets rotated if needed | If a secret was exposed or is being replaced, rotate it before deploy. |
| Feature flags configured | New flags exist in the flag service and are set to the correct default for production. |

### Dependencies

| Check | Details |
|-------|---------|
| Lock file committed | `pnpm-lock.yaml` / `package-lock.json` / `yarn.lock` is checked in and up to date. |
| No known vulnerabilities | Run `pnpm audit` (or equivalent). No critical or high severity issues. |
| No untested major version bumps | Major dependency upgrades have been tested in staging or a preview environment. |

### Performance

| Check | Details |
|-------|---------|
| No bundle size regressions | Compare bundle output before and after. Flag increases > 5%. |
| Lighthouse check on key pages | Run Lighthouse on landing page, main app page. No score drops > 5 points. |
| No N+1 queries | Review new database queries. Use eager loading or batching where needed. |

### Monitoring

| Check | Details |
|-------|---------|
| Error tracking configured | New features report errors to Sentry or equivalent. Source maps uploaded. |
| Alerts set | Critical paths have alerting. New endpoints included in uptime checks. |
| Logging adequate | Key operations log enough to debug issues without leaking sensitive data. |

## Rollback Plan

Define the rollback strategy before deploying. Don't deploy without one.

| Scenario | Action |
|----------|--------|
| Bug in application code | Revert the commit and redeploy the previous version. |
| Feature behaving unexpectedly | Toggle the feature flag off. No redeploy needed. |
| Database migration failure | Run the `down` migration. If irreversible, restore from the pre-deploy backup. |
| Third-party service outage | Activate fallback or circuit breaker. Communicate status to users. |

Steps for every rollback:
1. Identify the issue and confirm it's deploy-related.
2. Execute the appropriate action from the table above.
3. Verify the rollback resolved the issue (check error rates, run smoke tests).
4. Notify the team with a brief incident summary.

## Smoke Tests

Run these critical paths immediately after deploy. Fail any one and trigger the rollback plan.

| Path | What to verify |
|------|---------------|
| Auth flow | Sign up, sign in, sign out, password reset all complete without errors. |
| Main CRUD operations | Create, read, update, delete on the primary resource. Confirm data persists. |
| Payment flow (if applicable) | Test checkout with a test card. Verify webhook delivery. Confirm no double charges. |
| API health | Hit `/api/health`. Expect 200 with all services reporting healthy. |
| Email / notifications | Trigger a transactional email or notification. Confirm delivery. |
| Key integrations | Verify third-party API calls succeed (OAuth, webhooks, external data). |

## Deploy Strategy

Choose the right strategy based on risk and infrastructure.

| Strategy | How it works | When to use | Risk level |
|----------|-------------|-------------|------------|
| **Rolling** | Replace instances one at a time. | Low-risk changes, stateless services. | Low |
| **Blue-green** | Run two identical environments. Switch traffic after verification. | Database migrations, breaking API changes. | Medium |
| **Canary** | Route a small percentage of traffic to the new version. Monitor, then expand. | High-risk changes, large user base, uncertain performance impact. | Lowest |
| **Recreate** | Stop all old instances, start new ones. Brief downtime. | Dev/staging environments, or when downtime is acceptable. | Highest |

## Output Format

Present the checklist as a walkthrough. For each category:
1. List the checks with their current status.
2. Flag any items that fail or need attention.
3. Summarize at the end: ready to deploy, or blocked with specific items to resolve.
