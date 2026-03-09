---
name: db-schema
description: Design a database schema from product requirements. Postgres-first with Supabase awareness. Use when user says "design the database", "create schema", "data model", "database structure", "what tables do I need", "schema design", or needs to translate requirements into a relational schema.
user-invokable: true
args:
  - name: target
    description: The feature or requirements to design a schema for (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

# Database Schema Design

Design a Postgres database schema from product requirements. If `target` is provided, scope to that feature. Otherwise, ask the user what they're building.

## Step 1: Identify Entities and Relationships

Read the requirements (PRD, user stories, or conversation). Extract:

- **Nouns** = candidate tables (users, projects, comments, invoices)
- **Verbs** = candidate relationships (creates, belongs to, has many)
- **Adjectives/states** = candidate columns or enums (active, draft, published)

Ask clarifying questions if the requirements are ambiguous. Don't guess at business logic.

## Step 2: Design Tables

Follow these conventions on every table:

| Convention | Rule | Example |
|------------|------|---------|
| Table names | `snake_case`, plural | `user_profiles`, `org_members` |
| Column names | `snake_case` | `first_name`, `created_at` |
| Primary keys | `uuid`, auto-generated | `id uuid default gen_random_uuid() primary key` |
| Timestamps | On every table, non-nullable | `created_at timestamptz default now() not null` |
| Updated timestamp | On every table | `updated_at timestamptz default now() not null` |
| Foreign keys | Named explicitly, with `on delete` | `user_id uuid references users(id) on delete cascade` |
| Soft deletes | Use `deleted_at timestamptz` when needed | Only when business requires undo/recovery |
| Booleans | Prefix with `is_` or `has_` | `is_active`, `has_verified_email` |

### Postgres Type Recommendations

| Data | Use | Don't use | Why |
|------|-----|-----------|-----|
| Identifiers | `uuid` | `serial`, `integer` | Non-sequential, safe to expose, no collision across tables |
| Timestamps | `timestamptz` | `timestamp` | Always store timezone-aware. Convert on display. |
| Money | `numeric(12,2)` or `bigint` (cents) | `float`, `real` | Floating point causes rounding errors |
| Email | `text` with check constraint | `varchar(255)` | `text` is faster in Postgres. Constrain with `check`. |
| Short strings | `text` | `varchar(n)` | Postgres `text` has no performance penalty. Use `check(length(col) <= n)` if needed. |
| Status/enum | `text` with check, or Postgres `enum` | Magic integers | `check (status in ('draft','published','archived'))` |
| JSON data | `jsonb` | `json` | `jsonb` is indexable and more efficient |
| IP addresses | `inet` | `text` | Native type supports operations |
| Tags/arrays | `text[]` or junction table | CSV in text column | Arrays for simple cases, junction table for queryable tags |

## Step 3: Define Relationships

### One-to-Many

Put the foreign key on the "many" side:

```sql
create table posts (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references users(id) on delete cascade not null,
  title text not null,
  created_at timestamptz default now() not null,
  updated_at timestamptz default now() not null
);
```

### One-to-One

Same as one-to-many but add a unique constraint:

```sql
create table user_profiles (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references users(id) on delete cascade not null unique,
  bio text,
  avatar_url text,
  created_at timestamptz default now() not null,
  updated_at timestamptz default now() not null
);
```

### Many-to-Many

Use a junction table. Name it `<table_a>_<table_b>` in alphabetical order, or use a domain name if clearer:

```sql
create table project_members (
  project_id uuid references projects(id) on delete cascade not null,
  user_id uuid references users(id) on delete cascade not null,
  role text not null default 'member' check (role in ('owner','admin','member','viewer')),
  joined_at timestamptz default now() not null,
  primary key (project_id, user_id)
);
```

## Step 4: Add Indexes

Create indexes for:

| Index when | Example |
|------------|---------|
| Foreign keys (always) | `create index idx_posts_user_id on posts(user_id);` |
| Columns in `WHERE` clauses | `create index idx_posts_status on posts(status);` |
| Columns in `ORDER BY` | `create index idx_posts_created_at on posts(created_at desc);` |
| Unique lookups | `create unique index idx_users_email on users(email);` |
| Composite queries | `create index idx_posts_user_status on posts(user_id, status);` |
| Full-text search | `create index idx_posts_title_search on posts using gin(to_tsvector('english', title));` |

Don't over-index. Each index costs write performance. Start with foreign keys and obvious query patterns. Add more based on actual query plans.

## Step 5: Write the Migration

Put everything in a Supabase migration:

```bash
npx supabase migration new create_initial_schema
```

Structure the migration file:

```sql
-- 1. Create enums (if using Postgres enums)
-- 2. Create tables (parent tables first, then children)
-- 3. Create indexes
-- 4. Enable RLS on all tables
-- 5. Create RLS policies
-- 6. Create functions and triggers (e.g., updated_at trigger)

-- Updated_at trigger function (create once, reuse)
create or replace function update_updated_at()
returns trigger as $$
begin
  new.updated_at = now();
  return new;
end;
$$ language plpgsql;

-- Example table
create table users (
  id uuid default gen_random_uuid() primary key,
  email text not null unique,
  full_name text,
  avatar_url text,
  created_at timestamptz default now() not null,
  updated_at timestamptz default now() not null
);

-- Apply updated_at trigger
create trigger set_updated_at
  before update on users
  for each row execute function update_updated_at();

-- Enable RLS
alter table users enable row level security;

-- Indexes
create index idx_users_email on users(email);
```

## Step 6: Supabase-Specific Guidance

| Concern | Approach |
|---------|----------|
| Auth users | Supabase manages `auth.users`. Reference with `auth.uid()`. Create a `public.users` or `public.profiles` table that mirrors/extends it. |
| Auto-create profile | Use a trigger on `auth.users` insert to create a row in `public.profiles` |
| RLS | Enable on every table. Write policies immediately. See `supabase-setup` skill for patterns. |
| Realtime | Add tables to Supabase Realtime publication if live updates are needed: `alter publication supabase_realtime add table <table>;` |
| Type generation | Run `npx supabase gen types typescript --local` after every migration |

### Auto-create profile trigger

```sql
create or replace function public.handle_new_user()
returns trigger as $$
begin
  insert into public.profiles (user_id, email, full_name, avatar_url)
  values (
    new.id,
    new.email,
    new.raw_user_meta_data->>'full_name',
    new.raw_user_meta_data->>'avatar_url'
  );
  return new;
end;
$$ language plpgsql security definer;

create trigger on_auth_user_created
  after insert on auth.users
  for each row execute function public.handle_new_user();
```

## Schema Review Checklist

| Check | Pass? |
|-------|-------|
| Every table has `id uuid` primary key | |
| Every table has `created_at` and `updated_at` | |
| All foreign keys have `on delete` behavior defined | |
| All foreign keys have indexes | |
| RLS is enabled on every table | |
| No `varchar` without justification (use `text`) | |
| No `timestamp` without `tz` (use `timestamptz`) | |
| No floating point for money | |
| Junction tables have composite primary keys | |
| `updated_at` trigger is applied to all tables | |
| Migration runs cleanly on `supabase db reset` | |
| Types regenerated after migration | |
