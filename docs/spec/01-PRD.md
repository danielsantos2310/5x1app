# PRD — Product Requirements Document
Status: Draft | Owner: Product | Version 0.1

## Product problem
Volleyball clubs use fragmented spreadsheets, messages and calendars to administer rosters, event participation and attendance. 5x1 combines these four daily operations into one low-friction, phone-friendly experience.

## v1 objective
One real club, IVI, can create its club, manage multiple teams and memberships, schedule events, collect RSVPs with fair capacity enforcement, and record attendance. Each capability must be reusable by a second club without database redesign.

## Roles and scope
- **Club admin:** configure club and teams, approve members, manage club role assignments, create/cancel events, review attendance.
- **Coach:** manage events and attendance only for assigned team(s), view applicable roster.
- **Player:** view own team(s), join/request membership, see events, RSVP, view attendance where policy allows.
- **Guardian:** excluded from v1; access, approval and linked minor profiles in v2.
- A person may have more than one club role or team relationship; represent assignments explicitly rather than one global `role` field.

## Required v1 features and acceptance
| ID | Requirement | Acceptance criterion |
| --- | --- | --- |
| AUTH-01 | Email/password signup, login, reset | Reset works without account enumeration; verified auth session required for protected data |
| I18N-01 | English on screen; full pt-BR translation chosen in Settings | No pt-BR labels on screens; every first-release string has an English value; pt-BR covers all screens, errors, emails if used and accessibility labels; choice saved to profile |
| CLUB-01 | Create club with name, logo, timezone | Valid data persisted, creator becomes admin |
| CLUB-02 | Expiring/revocable invite and request approval | Expired/revoked token denied; requests cannot self-approve |
| TEAM-01 | Multiple teams per club | Club admin creates/edits team with `division` and `gender_category`; six IVI examples supported |
| ROST-01 | Assign members to one or more teams | No accidental cross-team access; duplicates rejected |
| ROLE-01 | Multiple roles by club and authorized team | Admin and coach permissions enforced server-side, not by hiding buttons |
| EVT-01 | Create/edit/cancel event for club or team | Fields: type, title, start/end, venue text/map URL, optional capacity (blank = no limit), RSVP cutoff (default 2 hours before start); social events may be `mixed` |
| RSVP-01 | Going / not going / maybe | One active status per user/event; response can be revised until cutoff |
| RSVP-02 | Waitlist capacity + automatic promotion | Never exceed capacity even under concurrent responses; promotion follows queue order; admins can promote, hold or remove waitlisted RSVPs, with audit; lowering capacity keeps current going RSVPs and warns the admin |
| ATT-01 | Mark present / absent / late | Admin, or coach for their own team; club-wide social events admin only. Only going RSVPs can be marked; waitlisted and maybe RSVPs must be promoted first; cancelled events are read-only; corrections at any time; every change audited; players see only their own history; no bulk "mark all present" in v1 |
| NOTE-01 | In-app notifications | Relevant new/change/cancel event notices visible to intended users |
| PWA-01 | Mobile-first installable PWA | Manifest/icons/HTTPS/installation tested; offline shell does not imply offline RSVP |
| ACCESS-01 | Accessible responsive screens | WCAG 2.2 AA acceptance checks |

## Four crucial v1 journeys
1. Admin signs up, creates IVI (or a club), configures team and invite link.
2. Player opens invite, authenticates and joins or requests approval; admin assigns team(s).
3. Coach/admin creates training, match or social event; player submits RSVP; capacity/waitlist updates live.
4. Coach/admin opens an event and records attendance with one-tap status controls.

## Success measurement
- P50 successful new-admin club + invite + first event time **<5 minutes**, timed from post-signup authenticated start; report sample size and P90.
- Player RSVP successful in **<=2 taps** from visible event card (not counting sign-in), with confirmation and offline errors surfaced.
- Coach records attendance for a **pre-agreed pilot roster size** in **<2 minutes**; choose fixed 12/16/20-player test before benchmarking.
- Reliability: proposed uptime 99.5% v1; main screens <2 seconds on defined midrange 4G conditions; realtime propagation <3 seconds when connected.
- Pilot exit: two weeks without blocking bugs; 8+ sessions with attendance during IVI four-week pilot.

## Outside v1
Payments, scoring/statistics, leagues, chat, public feed, drills, evaluations, guardian accounts, dedicated venue library, recurring events, native store apps and monetization.

## v2 candidate backlog
Guardian support; recurring events; notifications by email/push; registration forms; team/season history; maps and venue records; league modules; native apps if justified by usage.

## Open product decisions
- Are `maybe` RSVPs counted against capacity? **Decided 14 Oct 2026: no.**
- Are social events club-wide? **Proposed: yes, if `team_id` is NULL; team events otherwise.**
- Can non-team club members see a team-specific event? **Proposed: only if explicitly targeted or admin.**
- Can an attendance record exist for someone whose RSVP was not `going`? **Decided 14 Oct 2026: no. Walk-ins are not handled in the app.**
- Are profiles of minors supported in v1 without guardian interactions? Requires policy and legal assessment before onboarding minors.
