---
name: vercel-deploy
description: Deploy a project to Vercel with proper configuration, environment variables, domains, and preview deployments. Use when user says "deploy to Vercel", "set up Vercel", "configure deployment", "add a domain", "preview deploys", or needs to ship a project to production.
user-invokable: true
args:
  - name: target
    description: The project or specific config to set up (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

# Vercel Deployment

Deploy and configure a project on Vercel. If `target` specifies a particular concern (domains, env vars, etc.), focus there. Otherwise, do the full setup.

## Step 1: Install and Authenticate

```bash
pnpm add -g vercel
vercel login
```

If already installed, verify with `vercel --version`.

## Step 2: Link the Project

From the project root:

```bash
vercel link
```

Follow the prompts to connect to an existing Vercel project or create a new one. This creates a `.vercel/` directory locally (already in `.gitignore` by default).

## Step 3: Configure vercel.json

Create `vercel.json` in the project root only if you need non-default settings. Next.js projects usually need minimal config.

```json
{
  "framework": "nextjs",
  "buildCommand": "pnpm build",
  "installCommand": "pnpm install",
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        { "key": "X-Content-Type-Options", "value": "nosniff" },
        { "key": "X-Frame-Options", "value": "DENY" },
        { "key": "Referrer-Policy", "value": "strict-origin-when-cross-origin" }
      ]
    }
  ],
  "redirects": [],
  "rewrites": []
}
```

| Field | When to set |
|-------|-------------|
| `framework` | Auto-detected for Next.js. Set explicitly if detection fails. |
| `buildCommand` | Override if using a monorepo or custom build step |
| `installCommand` | Override if not using npm (e.g., `pnpm install`) |
| `headers` | Add security headers, CORS, caching |
| `redirects` | Domain migrations, URL restructuring |
| `rewrites` | Proxy API routes, SPA fallbacks |
| `regions` | Pin serverless functions to specific regions (e.g., `["iad1"]`) |

## Step 4: Environment Variables

### Set via CLI

```bash
# Add for all environments
vercel env add NEXT_PUBLIC_APP_URL

# Add for specific environments
vercel env add DATABASE_URL production
vercel env add DATABASE_URL preview
vercel env add DATABASE_URL development
```

### Environment Types

| Environment | When used | Notes |
|-------------|-----------|-------|
| **Production** | Main branch deploys | Use production database, real API keys |
| **Preview** | PR and branch deploys | Use staging database, test API keys |
| **Development** | `vercel dev` locally | Mirrors `.env.local` |

### Pull env vars to local

```bash
vercel env pull .env.local
```

This syncs your Vercel development env vars to `.env.local`. Never commit this file.

### Common env var issues

| Problem | Fix |
|---------|-----|
| Env var not available at build time | Ensure it's set for the correct environment (Production/Preview) |
| Client-side var is `undefined` | Prefix with `NEXT_PUBLIC_` |
| Changed env var, still old value | Redeploy. Env vars are baked in at build time. |
| Secret leaking to client | Remove `NEXT_PUBLIC_` prefix. Only server code can access non-prefixed vars. |

## Step 5: Custom Domains

### Add a domain

```bash
vercel domains add yourdomain.com
```

### Configure DNS

Vercel provides DNS records to set. Common patterns:

| Record type | Name | Value | Use case |
|-------------|------|-------|----------|
| `A` | `@` | `76.76.21.21` | Root domain |
| `CNAME` | `www` | `cname.vercel-dns.com` | www subdomain |
| `CNAME` | `app` | `cname.vercel-dns.com` | Custom subdomain |

### Redirect www to root (or vice versa)

Configure in Vercel Dashboard > Domains, or in `vercel.json`:

```json
{
  "redirects": [
    {
      "source": "/:path((?!api/).*)",
      "has": [{ "type": "host", "value": "www.yourdomain.com" }],
      "destination": "https://yourdomain.com/:path",
      "permanent": true
    }
  ]
}
```

## Step 6: Build Settings

### Framework presets

Vercel auto-detects Next.js. Override only when needed:

| Setting | Default (Next.js) | Override when |
|---------|-------------------|---------------|
| Build command | `next build` | Monorepo, custom scripts |
| Output directory | `.next` | Never change for Next.js |
| Install command | `npm install` | Using pnpm or yarn |
| Root directory | `/` | Monorepo (set to app directory) |
| Node.js version | 20.x | Need specific version |

### Monorepo setup

Set the root directory in Vercel project settings to the app's subdirectory:

```
Root Directory: apps/web
```

Or use `vercel.json` at the repo root:

```json
{
  "ignoreCommand": "npx turbo-ignore"
}
```

## Step 7: Preview Deployments

Every push to a non-production branch creates a preview deployment automatically.

### Branch-based previews

- Each PR gets a unique URL: `<project>-<branch>-<team>.vercel.app`
- Comment bots post the preview URL on PRs
- Preview deployments use "Preview" environment variables

### Protect preview deployments

In Vercel Dashboard > Settings > General > Password Protection, enable password or Vercel Authentication for preview URLs.

### Skip deployments for non-code changes

Add to `vercel.json`:

```json
{
  "ignoreCommand": "git diff --quiet HEAD^ HEAD -- . ':!docs' ':!*.md'"
}
```

## Step 8: Serverless and Edge Functions

### Serverless function config

Next.js API routes and Server Actions deploy as serverless functions by default.

Set runtime and region in the route file:

```ts
export const runtime = 'nodejs' // or 'edge'
export const maxDuration = 30   // seconds (default 10, max depends on plan)
export const preferredRegion = 'iad1'
```

### Edge vs. Serverless

| Feature | Serverless (`nodejs`) | Edge (`edge`) |
|---------|----------------------|---------------|
| Cold start | ~250ms | ~0ms |
| Runtime | Full Node.js | V8 (limited APIs) |
| Max duration | 10-300s (plan-dependent) | 30s |
| Use for | Database queries, heavy computation | Auth, redirects, geolocation |
| Node APIs | Full | Subset (no `fs`, limited `crypto`) |

## Step 9: Deploy

### Preview deploy (from any branch)

```bash
vercel
```

### Production deploy

```bash
vercel --prod
```

### Via Git (recommended)

Connect the Git repo in Vercel Dashboard. Every push to `main` auto-deploys to production. Every PR branch gets a preview.

## Troubleshooting

| Problem | Cause | Fix |
|---------|-------|-----|
| Build fails with module not found | Missing dependency | Check `package.json`. Run `pnpm install` and retry. |
| Build fails with type errors | TypeScript strict mode | Fix type errors locally first: `pnpm build` |
| 404 on dynamic routes | Missing `generateStaticParams` or wrong config | Add `export const dynamic = 'force-dynamic'` or implement `generateStaticParams` |
| API route returns 500 | Unhandled error or missing env var | Check Vercel function logs: `vercel logs <url>` |
| Slow cold starts | Large function bundle | Reduce dependencies. Use edge runtime where possible. |
| CORS errors | Missing headers | Add CORS headers in `vercel.json` or middleware |
| Deployment stuck | Build queue | Cancel and retry: `vercel --force` |

## Deployment Checklist

| Task | Status |
|------|--------|
| Vercel CLI installed and authenticated | |
| Project linked with `vercel link` | |
| `vercel.json` configured (if needed) | |
| Env vars set for Production, Preview, Development | |
| Custom domain added and DNS configured | |
| SSL certificate provisioned (automatic) | |
| Preview deployments working on PRs | |
| `vercel --prod` deploys successfully | |
| Security headers configured | |
| Function regions set (if latency-sensitive) | |
