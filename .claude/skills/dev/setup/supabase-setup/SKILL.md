---
name: supabase-setup
description: Initialize Supabase for a project including database schema, Row Level Security policies, authentication, storage buckets, and edge functions. Use when user says "set up Supabase", "add Supabase", "configure database", "add auth", "Supabase init", "RLS policies", or needs a backend with Supabase.
user-invokable: true
args:
  - name: target
    description: What to set up (auth, storage, schema, all) (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

# Supabase Setup

Initialize and configure Supabase for a project. If `target` is provided, focus on that area (auth, storage, schema). Default to "all" if omitted.

## Step 1: Install and Initialize

```bash
pnpm add @supabase/supabase-js
pnpm add -D supabase
npx supabase init
```

This creates a `supabase/` directory with config and a `migrations/` folder.

Link to a remote project (ask the user for their project ref if not provided):

```bash
npx supabase link --project-ref <project-ref>
```

## Step 2: Environment Variables

Add to `.env.local`:

```bash
NEXT_PUBLIC_SUPABASE_URL=https://<project-ref>.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=<anon-key>
SUPABASE_SERVICE_ROLE_KEY=<service-role-key>
```

| Variable | Exposure | Purpose |
|----------|----------|---------|
| `NEXT_PUBLIC_SUPABASE_URL` | Client + Server | API endpoint |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Client + Server | Public key for RLS-protected access |
| `SUPABASE_SERVICE_ROLE_KEY` | Server only | Bypasses RLS. Never expose to client. |

## Step 3: Create the Supabase Client

Create `src/lib/supabase/client.ts` (browser client):

```ts
import { createBrowserClient } from '@supabase/ssr'

export function createClient() {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
  )
}
```

Create `src/lib/supabase/server.ts` (server client):

```ts
import { createServerClient } from '@supabase/ssr'
import { cookies } from 'next/headers'

export async function createClient() {
  const cookieStore = await cookies()
  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() {
          return cookieStore.getAll()
        },
        setAll(cookiesToSet) {
          cookiesToSet.forEach(({ name, value, options }) =>
            cookieStore.set(name, value, options),
          )
        },
      },
    },
  )
}
```

Install the SSR helper:

```bash
pnpm add @supabase/ssr
```

## Step 4: Schema Design (Migrations)

Create migrations with:

```bash
npx supabase migration new <migration-name>
```

Follow these conventions in every migration:

- Use `uuid` for primary keys: `id uuid default gen_random_uuid() primary key`
- Add timestamps to every table: `created_at timestamptz default now() not null`, `updated_at timestamptz default now() not null`
- Use `snake_case` for all table and column names
- Use plural table names (`users`, `posts`, `comments`)
- Always enable RLS: `alter table <table> enable row level security;`

Apply migrations locally:

```bash
npx supabase db reset   # Resets local DB and applies all migrations
```

Push to remote:

```bash
npx supabase db push
```

## Step 5: Row Level Security (RLS)

Enable RLS on every table. Never leave a table without policies in production.

### Common RLS Patterns

| Pattern | Use when | Policy SQL |
|---------|----------|------------|
| **Owner access** | Users own their rows | `auth.uid() = user_id` |
| **Org-based access** | Users belong to an org | `auth.uid() in (select user_id from org_members where org_id = <table>.org_id)` |
| **Role-based access** | Different permission levels | `exists (select 1 from user_roles where user_id = auth.uid() and role = 'admin')` |
| **Public read** | Content visible to all | `true` (on SELECT only) |
| **Authenticated read** | Any logged-in user can read | `auth.uid() is not null` (on SELECT only) |

### Example: Owner-based CRUD

```sql
-- Users can read their own rows
create policy "Users read own data"
  on profiles for select
  using (auth.uid() = user_id);

-- Users can insert their own rows
create policy "Users insert own data"
  on profiles for insert
  with check (auth.uid() = user_id);

-- Users can update their own rows
create policy "Users update own data"
  on profiles for update
  using (auth.uid() = user_id)
  with check (auth.uid() = user_id);

-- Users can delete their own rows
create policy "Users delete own data"
  on profiles for delete
  using (auth.uid() = user_id);
```

### Example: Org-based Access

```sql
create policy "Org members can read"
  on projects for select
  using (
    exists (
      select 1 from org_members
      where org_members.org_id = projects.org_id
        and org_members.user_id = auth.uid()
    )
  );
```

### Example: Role-based Access

```sql
create policy "Admins can do anything"
  on settings for all
  using (
    exists (
      select 1 from user_roles
      where user_roles.user_id = auth.uid()
        and user_roles.role = 'admin'
    )
  );
```

## Step 6: Authentication

### Email + Password

Enabled by default. Create sign-up and login flows:

```ts
// Sign up
const { data, error } = await supabase.auth.signUp({
  email: 'user@example.com',
  password: 'securepassword',
})

// Sign in
const { data, error } = await supabase.auth.signInWithPassword({
  email: 'user@example.com',
  password: 'securepassword',
})
```

### OAuth Providers

Enable in Supabase Dashboard > Authentication > Providers. Common setup:

```ts
const { data, error } = await supabase.auth.signInWithOAuth({
  provider: 'google', // or 'github', 'apple', etc.
  options: {
    redirectTo: `${window.location.origin}/auth/callback`,
  },
})
```

Create an auth callback route at `src/app/auth/callback/route.ts`:

```ts
import { NextResponse } from 'next/server'
import { createClient } from '@/lib/supabase/server'

export async function GET(request: Request) {
  const { searchParams, origin } = new URL(request.url)
  const code = searchParams.get('code')

  if (code) {
    const supabase = await createClient()
    await supabase.auth.exchangeCodeForSession(code)
  }

  return NextResponse.redirect(origin)
}
```

### Magic Link

```ts
const { data, error } = await supabase.auth.signInWithOtp({
  email: 'user@example.com',
})
```

### Auth Middleware

Create `src/middleware.ts` to protect routes:

```ts
import { createServerClient } from '@supabase/ssr'
import { NextResponse, type NextRequest } from 'next/server'

export async function middleware(request: NextRequest) {
  let supabaseResponse = NextResponse.next({ request })
  const supabase = createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() {
          return request.cookies.getAll()
        },
        setAll(cookiesToSet) {
          cookiesToSet.forEach(({ name, value }) =>
            request.cookies.set(name, value),
          )
          supabaseResponse = NextResponse.next({ request })
          cookiesToSet.forEach(({ name, value, options }) =>
            supabaseResponse.cookies.set(name, value, options),
          )
        },
      },
    },
  )

  const { data: { user } } = await supabase.auth.getUser()

  if (!user && !request.nextUrl.pathname.startsWith('/auth')) {
    const url = request.nextUrl.clone()
    url.pathname = '/auth/login'
    return NextResponse.redirect(url)
  }

  return supabaseResponse
}

export const config = {
  matcher: ['/((?!_next/static|_next/image|favicon.ico|auth).*)'],
}
```

## Step 7: Storage Buckets

Create buckets in a migration or via CLI:

```sql
insert into storage.buckets (id, name, public)
values ('avatars', 'avatars', false);
```

Add storage policies:

```sql
-- Users can upload their own avatar
create policy "Users upload own avatar"
  on storage.objects for insert
  with check (
    bucket_id = 'avatars'
    and auth.uid()::text = (storage.foldername(name))[1]
  );

-- Users can read their own avatar
create policy "Users read own avatar"
  on storage.objects for select
  using (
    bucket_id = 'avatars'
    and auth.uid()::text = (storage.foldername(name))[1]
  );
```

Upload pattern in code:

```ts
const { data, error } = await supabase.storage
  .from('avatars')
  .upload(`${userId}/avatar.png`, file)
```

## Step 8: TypeScript Type Generation

Generate types from your database schema:

```bash
npx supabase gen types typescript --local > src/types/database.ts
```

Use the generated types with the client:

```ts
import { Database } from '@/types/database'

const supabase = createBrowserClient<Database>(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
)
```

Re-run type generation after every migration.

## Step 9: Edge Functions (Optional)

Create an edge function:

```bash
npx supabase functions new <function-name>
```

Deploy:

```bash
npx supabase functions deploy <function-name>
```

Use edge functions for webhooks, scheduled tasks, or complex server-side logic that doesn't fit in Next.js API routes.

## Setup Checklist

| Task | Status |
|------|--------|
| `@supabase/supabase-js` and `@supabase/ssr` installed | |
| `supabase init` ran, `supabase/` directory exists | |
| Project linked with `supabase link` | |
| Environment variables set in `.env.local` | |
| Browser client created (`src/lib/supabase/client.ts`) | |
| Server client created (`src/lib/supabase/server.ts`) | |
| Initial migration created with schema | |
| RLS enabled on all tables | |
| RLS policies written for every table | |
| Auth method configured (email/OAuth/magic link) | |
| Auth callback route created | |
| Middleware protects authenticated routes | |
| TypeScript types generated from schema | |
| `.env.example` updated (no secrets) | |
