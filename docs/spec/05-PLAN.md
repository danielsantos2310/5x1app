# PLAN — Delivery Roadmap and Implementation Sequence
Version 0.1 | Proposed solo/part-time project schedule

## Timeline and gates
| Phase | Proposed dates | Deliverables | Exit gate |
| --- | --- | --- | --- |
| 0: Specification | 14–31 Oct 2026 | PRD/TRD/FLOWS/DESIGN/PLAN/BACKEND/SECURITY/TESTING, ADR proposals | Product scope and open decisions signed off |
| 1: UX/UI | 1–21 Nov 2026 | Wireframes, bilingual designs, usability review | Four flows approved |
| 2: Foundation | 22 Nov–5 Dec 2026 | Git repository, CI, migrations, auth, RLS, i18n, PWA shell | Signup/login/language works on a phone |
| 3: Core build | 6 Dec 2026–16 Jan 2027 | Clubs, teams, roles, invites, events, RSVP/waitlist, attendance | Four core E2E flows pass |
| 4: IVI pilot | 19 Jan–13 Feb 2027 | Real pilot and issue fixes | 8+ attended sessions, 2 blocker-free weeks |
| 5: Release | 16 Feb–6 Mar 2027 | Accessibility, privacy review, localization QA, staging/prod | Checklist passes; invite-only v1 released |
| 6: Post-v1 | From Mar 2027 | Guardian analysis, recurring events, native feasibility | Prioritized by evidence |

Dates are estimates from the original supplied spec, not commitments. Actual duration is dependent on availability, security/legal review and pilot results.

## Design progress (updated 14 Oct 2026)
Phase 1 is running ahead of its planned dates; dates are not followed, work proceeds by agreement. Each item below links to its source.

| Deliverable | Status | Where |
| --- | --- | --- |
| Decisions register (34 decisions) | Done, kept current | `product-spec.md` |
| Low-fidelity wireframes: club setup, player RSVP, event waitlist, attendance | Done (v0.2), phone only | `docs/internal/wireframes/` |
| Design board: five core screens, palette, type, components, navigation (light and dark) | Approved, v0.2 | `docs/internal/design/5x1-design-board-v0.2.html` |
| Design tokens: palette (light and dark), contrast, typography, spacing, breakpoints | Approved | `docs/internal/design-tokens.md` |
| High-fidelity: player home, admin overview | Done (v0.1), review pending | `docs/internal/design/hifi/hifi-player-admin-v0.1.html` with `-light.png` and `-dark.png` captures |
| High-fidelity: admin approval with team assignment, pending player waiting | Done (v0.1), review pending | `docs/internal/design/hifi/approvals-v0.1.html` with `-light.png` and `-dark.png` captures |
| High-fidelity: event detail (admin, with waitlist promote, hold, remove), attendance | Done (v0.2), approved 15 Oct 2026 | `docs/internal/design/hifi/eventatt-v0.2.html` with `-light.png` and `-dark.png` captures (v0.1 kept) |
| High-fidelity: landing and sign-in | Done (v0.1), review pending | `docs/internal/design/hifi/landing-v0.1.html` with `-light.png` and `-dark.png` captures |
| Sitemap and navigation model | Approved for the pilot, 14 Oct 2026 | `docs/internal/sitemap.md` |
| Brand identity: hexagon mark, wordmark, palette, app icon, light and dark rules | Approved, 14 Oct 2026 (vector source from designer still to come) | `docs/internal/brand/5x1-brand.md` |
| Tablet (768px) and desktop (1280px) layouts | Approved, 15 Oct 2026 | `docs/internal/design/hifi/layouts-tablet-desktop-v0.1.html` |
| Clickable prototype: landing to attendance, RSVP and waitlist rules, approval, role switch | Built (v0.1), for review | `docs/internal/design/prototype/index.html` (see its README) |
| Five-user usability test (player, coach, admin tasks) | Not started | — |
| Developer handoff: component states, measurements, accessible labels | Not started | — |
| Clickable prototype and five-user usability test | Not started | — |
| Developer handoff (component variants, states, accessible labels) | Not started | — |

## Build dependencies
1. Repository, lint/typecheck, test pipeline and migration framework.
2. Database schema, role policy and complete club-isolation tests.
3. Auth and language/profile.
4. Club creation, team configuration, join flow and permissions.
5. Event CRUD with team-scoped visibility.
6. Concurrency-safe RSVP and waitlist transactions.
7. Attendance, notifications and reporting baseline.
8. Accessibility, mobile and security testing; backups/restore drill.
9. IVI pilot, fix, production release.

## Example sprint backlog
| Sprint | Focus | Priority |
| --- | --- | --- |
| S0 | Scope, ADRs, flows, design system | P0 |
| S1 | CI + auth + localization + schema/RLS | P0 |
| S2 | Club setup + invites + membership | P0 |
| S3 | Teams + divisions + rosters + coach assignment | P0 |
| S4 | Events + timezone validation + notifications | P0 |
| S5 | RSVP + waitlist and race-condition tests | P0 |
| S6 | Attendance + audit + pilot preparation | P0 |
| S7 | Pilot bugs, accessibility, security, translation | P0 |
| S8 | Release readiness + backups/restore + deploy | P0 |

## Milestone checks
- M0: Approved documents and roles/residency decisions.
- M1: User can create account, switch locale, access an installable mobile app.
- M2: Admin configures IVI with six distinct team records across applicable divisions.
- M3: RSVPs are atomic and isolate clubs; coaches cannot manipulate another team.
- M4: Pilot observations meet success targets and privacy requirements.
- M5: Release checklist completed; rollback and incident response defined.

## Risks
- Cross-club data leakage -> RLS and adversarial multi-tenant tests before feature build.
- Overselling regional compliance -> region and legal/vendor review before international rollout.
- Mis-modeled roles (coach also player) -> junction tables, no global singleton role.
- RSVP race conditions -> serialized database transaction; load tests.
- Complex admin screens -> task-timed mobile tests.
- Scope creep -> gate v2 features and do not mix with v1.

## Working ritual
Every task has requirement ID, owner, priority, complexity, definition of done, test and dependencies. Keep weekly decision log, changelog and demos. Write migration before client UI that depends on it.
