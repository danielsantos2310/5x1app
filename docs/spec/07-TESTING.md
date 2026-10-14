# TESTING — Quality Assurance and Acceptance
Version 0.1

## Layers
- Static: strict TypeScript, linting, dependency/security scanning, formatting, migration checks.
- Unit: date/time utilities, locale formatting, validation, waitlist decision semantics.
- Integration: Postgres constraints, RLS, invites, role updates, RPC transactions, notifications.
- E2E Playwright: signup + club; join; create event + RSVP; attendance; mobile responsive.
- Manual: screen reader, keyboard, iOS and Android install, offline messaging, real IVI pilot.

## Acceptance scenarios
| Test ID | Scenario | Must prove |
| --- | --- | --- |
| T-01 | New admin -> club -> invite -> event | Under 5 min on defined device/profile |
| T-02 | Player RSVP on event list/card | <=2 taps to success |
| T-03 | Coach attendance on fixed roster | Under 2 min, correct saved records |
| T-04 | Two users concurrently claim one remaining slot | Exactly one `going`, other `waitlist` |
| T-05 | Cancel Going with nonempty waitlist | Oldest eligible waitlisted promoted exactly once |
| T-06 | Coach attempts other team event edit | Denied on server + database |
| T-07 | User belonging to club A requests club B records | Denied at RLS layer |
| T-08 | Revoked/expired invitation | Rejected without membership mutation |
| T-09 | Daylight savings in Europe/Dublin | Correct UTC storage + local display |
| T-10 | Switch locale | All pages, labels, errors in pt-BR / en |
| T-11 | No network during RSVP | Clear failure; no false confirmation |
| T-12 | Remove membership with historical attendance | Privacy rules respected; admin reports consistent |
| T-13 | Keyboard + screen reader | Core flows operable; button states announced |
| T-14 | PWA install from supported phone browsers | Installable; offline shell and logout sane |

## Gate and evidence
CI must pass unit, RLS integration and four critical E2E tests. Pilot QA reports timing distribution (not a single best-case run), blocker count and attendance coverage. Keep documented severity/triage and release rollback steps. No release with known cross-club access vulnerability or minors privacy gap.

## Pilot plan
Recruit IVI admins, coaches and players from multiple teams; confirm consent and policies first. Run at least 8 sessions, evaluate P50/P90 performance, capture usability friction and fix blockers. Do not infer compliance from successful functional tests.
