---
name: monitor
description: Set up monitoring with error tracking, alerts, dashboards, and SLOs for production applications. Use when user says "set up monitoring", "add error tracking", "create alerts", "observability", "SLOs", "health checks", "how do I know if it's broken", or needs production visibility.
user-invokable: true
args:
  - name: target
    description: The service or feature to set up monitoring for (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

# Production Monitoring Setup

Set up monitoring for a production application. If a `target` is provided, scope recommendations to that service or feature. Cover all four pillars, then configure health checks, alerts, SLOs, and dashboards.

## The Four Pillars

Monitor all four. Missing any one creates a blind spot.

### 1. Errors

Track unhandled exceptions, failed API calls, and client-side errors. Don't wait for users to report them.

| What to capture | How |
|----------------|-----|
| Unhandled exceptions (server) | Global error handler reports to error tracking service. Include stack trace and request context. |
| Unhandled exceptions (client) | `window.onerror` and `onunhandledrejection` wired to error tracking. |
| Failed API calls | Log 4xx and 5xx responses with request path, status, and duration. |
| Client-side errors | React error boundaries catch render failures. Report with component tree. |

**Tool examples:** Sentry (full stack, source maps, release tracking), LogRocket (session replay + error context).

### 2. Performance

Measure response times and user experience. Use percentiles, not averages.

| Metric | Target | How to measure |
|--------|--------|---------------|
| API response time (p50) | < 200ms | Server-side timing middleware. |
| API response time (p95) | < 500ms | Same middleware, track percentile distribution. |
| API response time (p99) | < 1000ms | Same middleware. If p99 spikes, investigate outlier queries. |
| Largest Contentful Paint | < 2.5s | Real User Monitoring (RUM) or Lighthouse CI. |
| First Input Delay | < 100ms | RUM. |
| Cumulative Layout Shift | < 0.1 | RUM or Lighthouse CI. |

**Tool examples:** Vercel Analytics (zero-config for Next.js), Speedlify (self-hosted Lighthouse tracking).

### 3. Availability

Know when your service is down before your users do.

| What to check | Frequency | Alert if |
|--------------|-----------|----------|
| Health check endpoint | Every 30s | Two consecutive failures. |
| SSL certificate expiry | Daily | Less than 14 days remaining. |
| DNS resolution | Every 5m | Resolution fails or returns unexpected IP. |
| Key third-party services | Every 1m | Dependency returns errors for > 2 minutes. |

**Tool examples:** Better Uptime (status pages + incident management), Checkly (synthetic monitoring with Playwright).

### 4. Business Metrics

Technical metrics alone don't tell you if the product works.

| Metric | Why it matters |
|--------|---------------|
| Signups per hour/day | Detects registration flow breakage immediately. |
| Conversion rate | Drop signals checkout or onboarding issues. |
| Key feature usage | Confirms new features are actually being used. |
| Error rate per user action | Ties technical errors to user impact. |

**Tool examples:** PostHog (open-source product analytics, feature flags, session replay), Mixpanel (funnel and retention analysis).

## Health Check Endpoint

Create a `/api/health` endpoint that verifies real connectivity, not just "the server is running."

```typescript
// app/api/health/route.ts
import { NextResponse } from 'next/server'

export async function GET() {
  const checks: Record<string, 'ok' | 'fail'> = {}

  // Check database connectivity
  try {
    await db.query('SELECT 1')
    checks.database = 'ok'
  } catch {
    checks.database = 'fail'
  }

  // Check external services (cache, queue, etc.)
  try {
    await redis.ping()
    checks.cache = 'ok'
  } catch {
    checks.cache = 'fail'
  }

  const allHealthy = Object.values(checks).every((s) => s === 'ok')

  return NextResponse.json(
    { status: allHealthy ? 'healthy' : 'degraded', checks },
    { status: allHealthy ? 200 : 503 },
  )
}
```

Adapt the checks to the actual services in the stack. Return 503 if any check fails so load balancers and uptime monitors detect it.

## Alerting Strategy

Alert on actionable signals only. Noisy alerts get ignored.

| Signal | Severity | Notification channel | Response time |
|--------|----------|---------------------|---------------|
| Health check down (2+ consecutive) | Critical | PagerDuty / phone call | < 15 minutes |
| Error rate > 5% of requests | Critical | Slack #incidents + PagerDuty | < 15 minutes |
| Error rate > 1% of requests | Warning | Slack #alerts | < 1 hour |
| p95 response time > 1s | Warning | Slack #alerts | < 1 hour |
| SSL cert expiry < 14 days | Info | Slack #ops | < 1 day |
| Disk usage > 80% | Warning | Slack #ops | < 4 hours |
| Deploy completed | Info | Slack #deploys | No response needed |

**Rules for good alerting:**
- Don't alert on single transient errors. Use thresholds and windows (e.g., "error rate > 5% over 5 minutes").
- Every alert must have a clear owner and a documented first-response action.
- Review alert fatigue monthly. If an alert fires > 3 times without action, fix it or remove it.

## SLOs (Service Level Objectives)

Define SLOs for key services. SLOs set expectations; SLIs measure them.

| Service | SLI (what you measure) | SLO (target) | Error budget |
|---------|----------------------|--------------|-------------|
| API availability | % of requests returning non-5xx | 99.9% | 8.7 hours downtime / year |
| API latency | p95 response time | < 500ms | 5% of requests can exceed |
| Web vitals (LCP) | p75 LCP across all pages | < 2.5s | 25% of page loads can exceed |
| Data pipeline freshness | Time since last successful sync | < 15 minutes | 4 allowed SLO violations / month |

**How to use error budgets:**
- When the budget is healthy, ship fast and take calculated risks.
- When the budget is low, freeze non-critical deploys and focus on reliability.
- Track budget burn rate weekly. A sudden spike means something broke.

## Dashboard Essentials

Build a team dashboard with these panels. Keep it to one screen.

| Panel | What it shows | Why |
|-------|--------------|-----|
| Error rate (last 24h) | Errors per minute, with deploy markers | Correlate errors with deploys instantly. |
| Latency trends (last 24h) | p50, p95, p99 lines on one chart | Spot gradual degradation before it becomes critical. |
| Active users (real-time) | Current connected / active users | Context for error rates. 10 errors with 10 users is worse than 10 errors with 10,000. |
| Uptime status | Green/red for each monitored endpoint | Glanceable health. |
| Deploy history | Last 10 deploys with timestamp and author | Quick reference for "what changed?" |
| SLO burn rate | Error budget remaining for the period | Know when to slow down. |

## Incident Response

Use severity levels to set expectations and escalation.

| Level | Definition | Example | Response expectation |
|-------|-----------|---------|---------------------|
| **SEV1** | Service down or major data loss. Most users affected. | API returning 500 for all requests. Payment processing broken. | All hands. Respond in < 15 min. Communicate status every 30 min. |
| **SEV2** | Significant degradation. Some users affected. | Slow response times. One region down. Feature broken for subset of users. | On-call responds in < 30 min. Hourly status updates. |
| **SEV3** | Minor issue. Workaround exists. | Non-critical feature broken. Cosmetic bug in production. | Address within business hours. No status page update needed. |

**For every incident:**
1. Acknowledge the alert and declare severity.
2. Open an incident channel (or thread) for coordination.
3. Mitigate first, investigate second. Get the service back up, then find root cause.
4. Write a brief post-incident review: what happened, why, and what will prevent recurrence.
