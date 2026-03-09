---
name: security-scan
description: Check code for OWASP top 10 vulnerabilities including injection, XSS, auth issues, and secrets exposure. Use when user says "security audit", "check for vulnerabilities", "security scan", "is this secure", "OWASP check", "find security issues", or needs to verify code security before shipping.
user-invokable: true
args:
  - name: target
    description: The file, directory, or feature to scan (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

Scan code for OWASP Top 10 vulnerabilities. Report severity-rated findings with file:line references and fix recommendations. Flag what matters; skip what doesn't apply.

## Scan Categories

Work through each OWASP category. For each, search for the specific code patterns listed.

### 1. Injection (A03:2021)

| Pattern to find | Risk | What to verify |
|----------------|------|---------------|
| String concatenation in SQL queries | SQL injection | Use parameterized queries / prepared statements |
| Template literals in database calls | NoSQL injection | Use query builders or ORM methods with bound parameters |
| `exec()`, `spawn()`, `system()` with user input | Command injection | Validate/whitelist input, avoid shell execution |
| User input in LDAP filters | LDAP injection | Escape special characters, use parameterized filters |
| User input in regex constructors | ReDoS | Validate regex complexity, use re2 or set timeouts |
| `eval()`, `Function()`, `vm.runInContext()` | Code injection | Remove entirely; never evaluate user-controlled strings |

### 2. Broken Authentication (A07:2021)

| Pattern to find | Risk | What to verify |
|----------------|------|---------------|
| Plain-text password storage | Credential theft | Use bcrypt/scrypt/argon2 with proper work factor |
| Missing rate limiting on login endpoints | Brute force | Implement rate limiting and account lockout |
| Session tokens in URLs or localStorage | Session hijack | Use httpOnly secure cookies |
| JWT without expiration or with weak signing | Token forgery | Set short exp, use RS256 or ES256, rotate keys |
| Missing MFA on sensitive operations | Account takeover | Require step-up auth for destructive actions |
| Hardcoded credentials or default passwords | Unauthorized access | Use environment variables and secrets management |

### 3. Sensitive Data Exposure (A02:2021)

| Pattern to find | Risk | What to verify |
|----------------|------|---------------|
| API keys, tokens, passwords in source | Credential leak | Move to environment variables / secrets manager |
| Secrets in log output | Log exfiltration | Redact sensitive fields before logging |
| Missing TLS / HTTP used for sensitive data | Man-in-the-middle | Enforce HTTPS, use HSTS header |
| PII in error responses | Information disclosure | Return generic errors to clients, log details server-side |
| Sensitive data in client-side state | Browser exposure | Keep secrets server-side, minimize client-side PII |
| Missing encryption at rest | Data breach | Encrypt sensitive database columns and file storage |

### 4. Cross-Site Scripting — XSS (A03:2021)

| Pattern to find | Risk | What to verify |
|----------------|------|---------------|
| `dangerouslySetInnerHTML` | Stored/reflected XSS | Sanitize with DOMPurify, validate input is trusted |
| `innerHTML`, `outerHTML` assignments | DOM-based XSS | Use textContent or framework-safe rendering |
| Unescaped template interpolation in HTML | Reflected XSS | Use framework auto-escaping, never bypass it |
| URL parameters rendered without encoding | Reflected XSS | Encode output, validate URL parameters |
| `javascript:` URLs in href/src | XSS via navigation | Whitelist URL schemes (http, https, mailto) |
| User-controlled `src` on script/iframe tags | Remote code execution | Validate against allowlist of trusted origins |

### 5. Broken Access Control (A01:2021)

| Pattern to find | Risk | What to verify |
|----------------|------|---------------|
| Missing auth middleware on routes | Unauthorized access | Apply auth checks to every protected endpoint |
| Client-side-only permission checks | Privilege escalation | Enforce permissions server-side |
| Direct object reference without ownership check | IDOR | Verify requesting user owns / has access to resource |
| Missing role/scope validation | Privilege escalation | Check roles on every sensitive operation |
| Unrestricted file upload | Arbitrary file write | Validate file type, size, and store outside webroot |
| Path traversal in file operations | File system access | Normalize paths, reject `..`, use allowlists |

### 6. Security Misconfiguration (A05:2021)

| Pattern to find | Risk | What to verify |
|----------------|------|---------------|
| Debug mode enabled in production config | Information disclosure | Disable debug, verbose errors, stack traces in prod |
| Default credentials in config files | Unauthorized access | Change all defaults, enforce strong credentials |
| Missing security headers | Various attacks | See Security Headers Checklist below |
| Overly permissive CORS (`*`) | Cross-origin attacks | Restrict to specific trusted origins |
| Directory listing enabled | Information disclosure | Disable in web server configuration |
| Unnecessary services or endpoints exposed | Attack surface | Remove unused routes, admin panels, debug endpoints |

### 7. Server-Side Request Forgery — SSRF (A10:2021)

| Pattern to find | Risk | What to verify |
|----------------|------|---------------|
| User-supplied URLs passed to fetch/request | Internal network access | Validate against URL allowlist |
| URL redirects without validation | Open redirect / SSRF | Check destination against trusted domains |
| DNS rebinding potential | Firewall bypass | Resolve DNS before validation, pin IPs |
| Internal service URLs constructable by user | Metadata endpoint access | Block RFC 1918 ranges and cloud metadata IPs |

### 8. Dependency Vulnerabilities (A06:2021)

| Pattern to find | Risk | What to verify |
|----------------|------|---------------|
| Outdated packages with known CVEs | Exploitation of known flaws | Run `npm audit` / `pnpm audit` and review results |
| Unpinned dependency versions (`^`, `~`, `*`) | Supply chain attack | Pin exact versions or use lock files |
| Dependencies from untrusted sources | Malicious code | Verify package provenance, check download counts |
| Post-install scripts in dependencies | Arbitrary code execution | Audit scripts, use `--ignore-scripts` where safe |

## Security Headers Checklist

Verify the following headers are set on HTTP responses:

| Header | Recommended Value | Purpose |
|--------|------------------|---------|
| `Content-Security-Policy` | Strict policy, no `unsafe-inline` / `unsafe-eval` | Prevents XSS, injection |
| `Strict-Transport-Security` | `max-age=63072000; includeSubDomains; preload` | Forces HTTPS |
| `X-Content-Type-Options` | `nosniff` | Prevents MIME-type sniffing |
| `X-Frame-Options` | `DENY` or `SAMEORIGIN` | Prevents clickjacking |
| `Referrer-Policy` | `strict-origin-when-cross-origin` | Limits referrer leakage |
| `Permissions-Policy` | Disable unused features (camera, microphone, geolocation) | Reduces attack surface |
| `X-XSS-Protection` | `0` (rely on CSP instead) | Deprecated but set explicitly |
| `Cache-Control` | `no-store` on sensitive responses | Prevents caching of secrets |

## Output Format

Report findings organized by severity:

### Finding Format

For each issue:

- **Severity**: Critical / High / Medium / Low
- **OWASP Category**: Which category (e.g., A01 Broken Access Control)
- **Location**: `file:line` reference
- **Issue**: What was found (one sentence)
- **Evidence**: The specific code pattern or configuration
- **Fix**: Concrete recommendation with code example
- **References**: Relevant CWE number or documentation link

### Summary

After all findings:

- **Total count** by severity and OWASP category
- **Top 3 risks** to address immediately
- **Attack surface assessment** (one paragraph)
- **Headers status** (pass/fail for each security header)

**NEVER**:
- Report theoretical issues without evidence in the actual code
- Skip a category because it "probably doesn't apply" — scan every one
- Provide a fix that introduces a different vulnerability
- Report low-severity style issues in a security scan (stay focused)
- Assume a framework handles security automatically without verifying configuration
