---
name: env-config
description: Set up environment variables across local, staging, and production with validation and documentation. Use when user says "set up env vars", "environment variables", "configure secrets", ".env setup", "env validation", or needs to manage configuration across environments.
user-invokable: true
args:
  - name: target
    description: The service or feature needing env configuration (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

# Environment Variable Configuration

Set up environment variables with validation, documentation, and secure handling across environments. If `target` names a service (Supabase, Stripe, etc.), scope to that service's vars. Otherwise, set up the full env system.

## Step 1: File Structure

Create these files in the project root:

| File | Purpose | Committed to git? |
|------|---------|-------------------|
| `.env.local` | Local development overrides. Highest priority. | No |
| `.env.development` | Defaults for `next dev` | Yes (no secrets) |
| `.env.production` | Defaults for `next build` | Yes (no secrets) |
| `.env.example` | Template documenting every variable | Yes |
| `.env.test` | Test environment overrides | Yes (no secrets) |

### Load order (Next.js)

Next.js loads env files in this order (later files override earlier):

1. `.env` (all environments)
2. `.env.local` (all environments, git-ignored)
3. `.env.[environment]` (e.g., `.env.development`)
4. `.env.[environment].local` (e.g., `.env.development.local`, git-ignored)

## Step 2: Naming Conventions

| Rule | Example | Why |
|------|---------|-----|
| `SCREAMING_SNAKE_CASE` | `DATABASE_URL` | Standard convention |
| `NEXT_PUBLIC_` prefix for client-side | `NEXT_PUBLIC_APP_URL` | Next.js exposes only prefixed vars to the browser |
| Group by service | `SUPABASE_URL`, `SUPABASE_ANON_KEY` | Easy to find related vars |
| No `NEXT_PUBLIC_` for secrets | `STRIPE_SECRET_KEY` (not `NEXT_PUBLIC_STRIPE_SECRET_KEY`) | Never expose secrets to the client |

### Common variable groups

| Service | Variables | Client-safe? |
|---------|-----------|-------------|
| **App** | `NEXT_PUBLIC_APP_URL`, `NEXT_PUBLIC_APP_NAME` | Yes |
| **Supabase** | `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY` | URL and anon key: yes. Service role: no. |
| **Stripe** | `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY`, `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET` | Publishable: yes. Others: no. |
| **Auth** | `NEXTAUTH_SECRET`, `NEXTAUTH_URL` | No |
| **Email** | `RESEND_API_KEY`, `EMAIL_FROM` | No |
| **Analytics** | `NEXT_PUBLIC_POSTHOG_KEY`, `NEXT_PUBLIC_POSTHOG_HOST` | Yes |

## Step 3: Validate with t3-env

Install t3-env for runtime validation:

```bash
pnpm add @t3-oss/env-nextjs zod
```

Create `src/env.ts`:

```ts
import { createEnv } from '@t3-oss/env-nextjs'
import { z } from 'zod'

export const env = createEnv({
  // Server-side variables (never exposed to client)
  server: {
    DATABASE_URL: z.string().url(),
    SUPABASE_SERVICE_ROLE_KEY: z.string().min(1),
    STRIPE_SECRET_KEY: z.string().startsWith('sk_'),
    STRIPE_WEBHOOK_SECRET: z.string().startsWith('whsec_'),
    NEXTAUTH_SECRET: z.string().min(1),
    NEXTAUTH_URL: z.string().url(),
  },

  // Client-side variables (exposed to browser)
  client: {
    NEXT_PUBLIC_APP_URL: z.string().url(),
    NEXT_PUBLIC_SUPABASE_URL: z.string().url(),
    NEXT_PUBLIC_SUPABASE_ANON_KEY: z.string().min(1),
    NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY: z.string().startsWith('pk_'),
  },

  // Map env vars to the schema
  runtimeEnv: {
    DATABASE_URL: process.env.DATABASE_URL,
    SUPABASE_SERVICE_ROLE_KEY: process.env.SUPABASE_SERVICE_ROLE_KEY,
    STRIPE_SECRET_KEY: process.env.STRIPE_SECRET_KEY,
    STRIPE_WEBHOOK_SECRET: process.env.STRIPE_WEBHOOK_SECRET,
    NEXTAUTH_SECRET: process.env.NEXTAUTH_SECRET,
    NEXTAUTH_URL: process.env.NEXTAUTH_URL,
    NEXT_PUBLIC_APP_URL: process.env.NEXT_PUBLIC_APP_URL,
    NEXT_PUBLIC_SUPABASE_URL: process.env.NEXT_PUBLIC_SUPABASE_URL,
    NEXT_PUBLIC_SUPABASE_ANON_KEY: process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY,
    NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY:
      process.env.NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY,
  },

  // Skip validation in Docker builds or CI where env vars aren't needed
  skipValidation: !!process.env.SKIP_ENV_VALIDATION,
})
```

### Usage

Import `env` instead of accessing `process.env` directly:

```ts
// Good — validated and typed
import { env } from '@/env'
const url = env.NEXT_PUBLIC_APP_URL

// Bad — unvalidated, untyped, silent failure
const url = process.env.NEXT_PUBLIC_APP_URL
```

This crashes at startup if required variables are missing, instead of failing silently at runtime.

## Step 4: Generate .env.example

Create `.env.example` with every variable, no real values, and comments:

```bash
# App
NEXT_PUBLIC_APP_URL=http://localhost:3000

# Supabase
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key

# Stripe
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...

# Auth
NEXTAUTH_SECRET=generate-with-openssl-rand-base64-32
NEXTAUTH_URL=http://localhost:3000

# Email
RESEND_API_KEY=re_...
EMAIL_FROM=onboarding@yourdomain.com
```

Keep `.env.example` in sync whenever you add or remove a variable.

## Step 5: Secrets Management

### Rules

| Rule | Enforcement |
|------|-------------|
| Never commit `.env.local` | Listed in `.gitignore` by default |
| Never commit real API keys | Use `.env.example` with placeholder values |
| Never log env vars | Redact in error handlers |
| Rotate compromised keys immediately | Revoke in provider dashboard, update in platform |
| Use platform env vars for production | Vercel, Railway, Fly.io all have env var management |

### Generate secrets

```bash
# Generate a random secret (e.g., for NEXTAUTH_SECRET)
openssl rand -base64 32
```

### Production env vars

Set via your hosting platform, never in files:

```bash
# Vercel
vercel env add STRIPE_SECRET_KEY production

# Railway
railway variables set STRIPE_SECRET_KEY=sk_live_...
```

## Step 6: Adding a New Env Var Checklist

Follow this checklist every time you add a new environment variable:

| Step | Action |
|------|--------|
| 1 | Add to `src/env.ts` schema (server or client) |
| 2 | Add to `runtimeEnv` mapping in `src/env.ts` |
| 3 | Add to `.env.local` with real value |
| 4 | Add to `.env.example` with placeholder value |
| 5 | Add to `.env.development` if it has a safe default |
| 6 | Set in Vercel/platform for production and preview |
| 7 | Update any documentation that lists env vars |
| 8 | Import from `@/env` in code (not `process.env`) |

## Common Patterns

### Feature flags via env vars

```ts
// In env.ts
server: {
  ENABLE_NEW_BILLING: z.enum(['true', 'false']).default('false'),
}

// In code
if (env.ENABLE_NEW_BILLING === 'true') {
  // new billing logic
}
```

### Per-environment API URLs

```bash
# .env.development
NEXT_PUBLIC_API_URL=http://localhost:3001

# .env.production
NEXT_PUBLIC_API_URL=https://api.yourdomain.com
```

### Optional variables

```ts
// In env.ts — use .optional() for non-required vars
server: {
  SENTRY_DSN: z.string().url().optional(),
  ANALYTICS_ID: z.string().optional(),
}
```

## Debugging Env Issues

| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| `undefined` in browser | Missing `NEXT_PUBLIC_` prefix | Rename with prefix |
| `undefined` on server | Variable not set in current environment | Check `.env.local` and platform settings |
| Old value after change | Next.js caches env at build time | Restart dev server or redeploy |
| Validation error at startup | Missing or malformed variable | Check `src/env.ts` schema against actual values |
| Works locally, fails in CI | CI doesn't have `.env.local` | Set vars in CI environment settings or use `SKIP_ENV_VALIDATION=1` for build-only steps |
