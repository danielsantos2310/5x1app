# Software development security checklist — 5x1 status

Date: 21 Oct 2026. No backend code exists yet, so "Covered" means the spec requires it and the build must test it. "Partial" means the spec names the idea but leaves a detail open. "Missing" means the spec is silent. Phase 2 (foundation, from 22 Nov) must close every Partial and Missing item, or record why not, before the first feature build.

## 1. Authentication and authorization
| Item | Status | Where / action |
| --- | --- | --- |
| Password hashing (bcrypt, Argon2 or PBKDF2 with salt) | Partial | AUTH-01 covers email/password. Supabase Auth hashes passwords; confirm the algorithm at build time and record it in an ADR. |
| MFA | Partial | 08-SECURITY says MFA for admins is recommended. Decide: mandatory for admins, optional for others. |
| Short-lived tokens, strong signing keys | Partial | Not specified. Set session and refresh lifetimes and key rotation in the auth ADR. |
| Permission checks at every endpoint and object (no ID guessing) | Covered | 08-SECURITY: RLS default deny, tests against guessed identifiers and mixed memberships. |

## 2. Communication and storage
| Item | Status | Where / action |
| --- | --- | --- |
| HTTPS with redirect, TLS 1.2 or higher | Partial | 08-SECURITY: enforce HTTPS. Add the redirect and minimum TLS version to the deployment config. |
| Secure cookies (HttpOnly, Secure, SameSite) | Partial | Cookies named; SameSite value not set. Set Lax or Strict in the auth ADR. CSRF is named in 08-SECURITY. |
| Encryption at rest | Covered | 08-SECURITY: platform encryption at rest and in transit. Open decision: field-level encryption for emergency contact and address (decision 43). |

## 3. Data validation and input
| Item | Status | Where / action |
| --- | --- | --- |
| Parameterized queries or ORM | Missing | Not stated. Add to 07-TESTING static checks: no string-built SQL. |
| Server-side validation | Partial | Validation rules exist for events (timezone, capacity). Make it a rule for every endpoint, not only events. |
| Output escaping (XSS) | Covered | React escapes by default; the prototype now escapes user-entered values. Ban raw HTML insertion in lint rules. |
| Protected uploads (size, magic numbers, storage outside execution path) | Missing | Photos and club logos use storage. Add size limit, magic-number check and private storage bucket to 06-BACKEND. |

## 4. Configuration and secrets
| Item | Status | Where / action |
| --- | --- | --- |
| No secrets in code | Covered | 08-SECURITY: secrets only in runtime env; scan for leaked keys. |
| .env files in .gitignore | Missing | Add .gitignore in repo setup (phase 2). |
| Debug mode off in production | Missing | Add to the release checklist (phase 5). |

## 5. Monitoring and error handling
| Item | Status | Where / action |
| --- | --- | --- |
| Generic error messages | Missing | Not stated. Add to 08-SECURITY: no stack traces or internal details to users. |
| Audit logs for sensitive actions | Covered | 06-BACKEND: audit_events table; decision 21 for attendance corrections. |
| Log masking | Covered | 08-SECURITY: do not log passwords, tokens, email content or full youth profiles. |

## 6. Infrastructure and dependencies
| Item | Status | Where / action |
| --- | --- | --- |
| Dependency scanning (npm audit, Snyk) | Covered | 07-TESTING: dependency and security scanning in static checks. |
| Rate limiting on login and password reset | Partial | Rate limiting is named for invites and RSVP. Add login and password reset explicitly. |
| Security headers (CSP, HSTS, nosniff) | Missing | Not stated. Add to the deployment config in phase 2. |

## Summary
20 items: Covered 7, Partial 7, Missing 6. Close every Partial and Missing item in phase 2.
