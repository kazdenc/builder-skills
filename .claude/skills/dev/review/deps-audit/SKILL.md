---
name: deps-audit
description: Audit project dependencies for outdated packages, known vulnerabilities, unused imports, and bloated bundles. Use when user says "audit dependencies", "check for vulnerabilities", "update packages", "unused dependencies", "bundle size", "dependency cleanup", or needs to maintain healthy dependencies.
user-invokable: true
args:
  - name: target
    description: The package.json or lock file to audit (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

Audit project dependencies and produce a categorized report with a prioritized upgrade plan. Surface vulnerabilities, unused packages, and bundle bloat.

## Step 1: Read Package Manifest

Read `package.json` (or the specified manifest file). Catalog:

- All `dependencies` and their pinned/range versions
- All `devDependencies` and their pinned/range versions
- Any `peerDependencies` or `optionalDependencies`
- Package manager in use (npm, pnpm, yarn) from lock file presence
- Node/runtime engine constraints if declared

## Step 2: Check for Vulnerabilities

Run the appropriate audit command for the detected package manager:

| Package Manager | Command | Notes |
|----------------|---------|-------|
| npm | `npm audit --json` | Parse JSON output for severity counts |
| pnpm | `pnpm audit --json` | Similar JSON structure |
| yarn (v1) | `yarn audit --json` | NDJSON output, parse line by line |
| yarn (berry) | `yarn npm audit --json` | Different output format |

For each vulnerability found, capture:

- Package name and installed version
- Severity (critical, high, moderate, low)
- CVE identifier if available
- Fix available (yes/no) and which version resolves it
- Whether it is a direct or transitive dependency

## Step 3: Identify Outdated Packages

Run `npm outdated --json` (or equivalent) and group results:

| Update Type | Risk Level | Action |
|-------------|-----------|--------|
| **Patch** (1.2.3 → 1.2.4) | Low | Safe to update; bug fixes only |
| **Minor** (1.2.3 → 1.3.0) | Low–Medium | Usually safe; new features, no breaking changes |
| **Major** (1.2.3 → 2.0.0) | Medium–High | Breaking changes expected; read changelog first |
| **Deprecated** | High | Package abandoned; find replacement |

Flag packages more than 2 major versions behind as high priority.

## Step 4: Find Unused Dependencies

Scan the codebase for actual import/require usage of each declared dependency:

- Search for `import ... from '<package>'` and `require('<package>')` patterns
- Check framework config files that reference packages implicitly (Babel, PostCSS, ESLint, Tailwind plugins)
- Check scripts in `package.json` that invoke CLI tools
- Check for `@types/*` packages whose corresponding runtime package is used

Classify as:

| Status | Meaning |
|--------|---------|
| **Unused** | No import, require, or config reference found anywhere |
| **Possibly unused** | Only referenced in commented-out code or unused files |
| **DevDependency misplaced** | Listed in `dependencies` but only used in tests/build |
| **Dependency misplaced** | Listed in `devDependencies` but imported in production code |

## Step 5: Identify Duplicates and Bundle Bloat

Check for common bundle-bloating patterns:

### Bloated Packages and Lighter Alternatives

| Heavy Package | Size | Alternative | Size | Notes |
|--------------|------|------------|------|-------|
| `moment` | ~290 kB | `dayjs` | ~7 kB | Drop-in API compatible with plugin system |
| `lodash` (full) | ~530 kB | Native ES methods / `lodash-es` cherry-pick | 0–5 kB | Most lodash utilities have native equivalents |
| `axios` | ~29 kB | Native `fetch` | 0 kB | Built into all modern runtimes |
| `uuid` | ~12 kB | `crypto.randomUUID()` | 0 kB | Built into Node 19+ and modern browsers |
| `classnames` | ~1 kB | Template literals or `clsx` | ~0.5 kB | `clsx` is smaller and faster |
| `underscore` | ~60 kB | Native ES methods | 0 kB | Fully replaceable with modern JS |
| `request` | ~200 kB | Native `fetch` or `undici` | 0–30 kB | `request` is deprecated |
| `bluebird` | ~80 kB | Native Promises | 0 kB | Native promises are sufficient in modern runtimes |
| `chalk` (v5+) | ~25 kB | `picocolors` | ~3 kB | Lighter, faster, no dependencies |
| `express` | ~210 kB | `fastify` or `hono` | ~50–80 kB | Better performance and smaller footprint |
| `left-pad` / trivial utils | varies | Inline implementation | 0 kB | Avoid micro-packages for trivial logic |

Also check for:

- **Duplicate packages** at different versions in the lock file
- **Multiple packages** serving the same purpose (e.g., both `axios` and `node-fetch`)
- **Large devDependencies** that could be replaced (e.g., `ts-node` → `tsx`)

## Step 6: Generate Upgrade Plan

Prioritize actions in this order:

| Priority | Category | Rationale |
|----------|----------|-----------|
| 1 | **Security fixes** (critical/high) | Active vulnerability; patch immediately |
| 2 | **Security fixes** (moderate/low) | Lower risk but still exposed |
| 3 | **Deprecated packages** | Replace before ecosystem support disappears |
| 4 | **Unused dependencies** | Remove to reduce attack surface and install time |
| 5 | **Bundle bloat replacements** | Swap heavy packages for lighter alternatives |
| 6 | **Major updates** (with breaking changes) | Schedule with time for testing and migration |
| 7 | **Minor/patch updates** | Batch and apply; lowest risk |

For each upgrade action, include:

- Package name and current → target version
- What changes (link to changelog or release notes when possible)
- Breaking changes to watch for
- Estimated effort (quick patch, moderate migration, significant rewrite)

## Output Report

Structure the final report with these sections:

### Vulnerability Summary
Total counts by severity. List each critical and high finding with CVE and fix version.

### Outdated Packages
Table grouped by update type (patch, minor, major, deprecated).

### Unused Dependencies
List with evidence of non-use. Separate confirmed-unused from possibly-unused.

### Bundle Optimization Opportunities
Heavy packages with recommended alternatives and estimated size savings.

### Prioritized Upgrade Plan
Ordered action list following the priority table above.

### Health Score
Provide an overall dependency health assessment:

| Metric | Status |
|--------|--------|
| Known vulnerabilities | Count by severity |
| Outdated packages | % of deps behind latest |
| Unused packages | Count |
| Bundle bloat candidates | Count and estimated savings |
| Lock file present | Yes / No |
| Engine constraints declared | Yes / No |

**NEVER**:
- Recommend removing a dependency without verifying it is truly unused (check config files, scripts, and implicit usage)
- Suggest upgrading a package without noting breaking changes in major versions
- Ignore transitive vulnerabilities — they are still your problem
- Recommend `npm audit fix --force` without reviewing what it changes
- Skip the lock file — duplicates and transitive issues live there
