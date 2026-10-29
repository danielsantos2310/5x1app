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

## 7. Code check, 29 Oct 2026 (eight-item review)
Scope: tracked files at commit 1946b51 (prototype HTML and Markdown only, no server code, no packages) and the 5x1 Supabase project.

| # | Item | Status | Evidence and action |
| --- | --- | --- | --- |
| 1 | Data validation and sanitisation | Partial | Client-side rules for name, email and password; escaping of names and image links (fixed 27 Oct). No server validation exists yet. Phase 2: validate on the server for every endpoint. |
| 2 | Parameterised queries | Not applicable yet | No query code in the repo. Phase 2: no string-built SQL, enforced in lint or review. |
| 3 | Secrets in environment variables | Not yet | No secrets found in tracked files or history. No `.gitignore`, so a `.env` file could be committed by mistake. Phase 2: add `.gitignore` first. |
| 4 | Secure error handling | Not applicable yet | Prototype shows friendly messages only. Phase 2: generic user messages, detailed server logs. |
| 5 | Least privilege | Partly verified | Supabase security advisor returned no findings (29 Oct). RLS enabled on all 10 public tables (earlier check). Policies not yet verified: the project is now INACTIVE (paused), so the policy query could not run. Action: restore the project, then check `pg_policies`. |
| 6 | Hashing and encryption | Not implemented | Prototype does not store passwords. Supabase Auth hashes passwords; confirm the algorithm in the auth ADR. Field-level encryption for emergency contacts is open (decision 43). |
| 7 | SAST | Not configured | No CI, linter or scanner. Phase 2: Semgrep or CodeQL in CI. |
| 8 | Third-party dependencies | Not applicable yet | No package files. Prototype loads nothing from third parties. Phase 2: Dependabot and `npm audit` once packages exist. |

Open action: restore the 5x1 Supabase project (currently INACTIVE) before the policy check. The project was active on 21 Oct.
