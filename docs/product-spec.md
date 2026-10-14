# 5x1 — Product Spec & Roadmap

2026-10-14 · Daniel Santos

## Overview and goals

5x1 is a free web and mobile app that lets a volleyball club run its members, events, RSVPs, and attendance in one place. v1 is built for one club first, our own, and is designed so other clubs can join later without a rewrite.

Goals for v1:

- Run one club end to end: roster, events, RSVP, and attendance.
- Work in the browser and on phones from one codebase.
- Ship in English, with Portuguese available from the first release.

Proposed success criteria (targets to confirm):

- An admin can create a club, invite members, and schedule an event in under 5 minutes.
- A member can RSVP in two taps or fewer.
- An admin can record attendance for a session in under 2 minutes.

## Users and roles

v1 has two primary roles, club admin and coach, and two secondary roles, player and guardian. Players are the largest group, so they must be able to RSVP without friction.

| Role | Who | What they need in v1 |
| --- | --- | --- |
| Club admin | Runs the club | Manage the roster, create events, see RSVPs and attendance, approve join requests |
| Coach | Leads training and matches | Create and view events for their team, record attendance, see who is coming |
| Player | Plays for the club | Join a club, see upcoming events, RSVP, see the team list |
| Guardian | Parent of a player under 18 | See a linked child's events and RSVPs, approve RSVPs (deferred to v2 unless needed) |

A person can hold more than one role, for example a coach who is also a player. Role is set per club, not globally.

Open question: guardians are a v2 feature unless the first club needs them sooner (see Open questions).

## Scope for v1

v1 covers the four modules you named, plus the minimum needed to make them usable: sign-in, a club, and roles.

**In scope (must-have)**

- Accounts: sign up, log in, reset password, English and Portuguese UI.
- Club: create a club, name and logo, invite by link, approve join requests.
- Roster: list members, assign roles (admin, coach, player), remove members.
- Events: create a session with title, type (match, training, social), date and time, venue as free text, capacity, and RSVP deadline.
- RSVP: members mark going, not going, or maybe; see the live list of who is going; waitlist when full.
- Attendance: admin or coach marks each RSVP as present, absent, or late after the session.
- Basic notifications: in-app list of new events and RSVP changes. Email is optional in v1.
- Responsive web app that works on phones and can be added to the home screen (PWA).

**Explicit non-goals for v1**

- Payments of any kind (not yet).
- Team chat, social feed, polls.
- Drills library, training plans, player evaluations, feedback.
- Guardian and family features.
- Leagues, match scoring, statistics.
- Native app store releases (iOS and Android). The PWA covers mobile first; native comes after v1 is stable.
- Monetization and paid plans.

**Deferred to v2 or later:** venue library with photos and maps, recurring events, registration forms, attendance streaks, and additional languages beyond Portuguese.

## Core user flows

Four flows carry v1. Each one should work on a phone in under a minute.

1. **Create a club.** Admin signs up, names the club, uploads a logo (optional), and gets an invite link.
2. **Join a club.** Player opens the invite link, signs up or logs in, and either joins at once or sends a request that the admin approves.
3. **Schedule and RSVP.** Admin creates an event. Members see it in their list, tap going, not going, or maybe, and the list updates for everyone. If the event is full, the next person joins the waitlist and is promoted when a spot opens.
4. **Record attendance.** After the session, the coach opens the event, sees the RSVP list, and marks each person present, absent, or late. The attendance record stays on the event for later reports.

## Functional requirements

Each requirement has an ID so we can refer to it in tasks and tests. All are must-have for v1 unless marked otherwise.

| ID | Module | Requirement |
| --- | --- | --- |
| AUTH-1 | Accounts | A user can sign up and log in with email and password. |
| AUTH-2 | Accounts | A user can reset a forgotten password by email. |
| AUTH-3 | Accounts | A user chooses English or Portuguese; the choice is saved to their profile. |
| CLUB-1 | Club | An admin creates a club with a name, optional logo, and timezone. |
| CLUB-2 | Club | An admin creates an invite link that can be expired or revoked. |
| CLUB-3 | Club | A user can request to join; an admin approves or declines. |
| ROST-1 | Roster | An admin or coach sees all members with their roles. |
| ROST-2 | Roster | An admin assigns or removes roles (admin, coach, player). |
| EVT-1 | Events | An admin or coach creates an event: title, type, date and time, venue, capacity, RSVP deadline. |
| EVT-2 | Events | An admin or coach edits or cancels an event; members are notified of changes. |
| RSVP-1 | RSVP | A member sets going, not going, or maybe, and can change it before the deadline. |
| RSVP-2 | RSVP | When capacity is reached, further RSVPs go to a waitlist and are promoted in order. |
| ATT-1 | Attendance | A coach marks each going member as present, absent, or late. |
| ATT-2 | Attendance | Attendance per member is viewable across events. (Could-have for v1.) |
| NOTE-1 | Notifications | A user sees an in-app list of new events and RSVP changes. |
| VEN-1 | Venues | An admin enters a venue as free text with an optional map link. |
| PWA-1 | Platform | The app installs to the home screen and works on phones. |

## Data model

The core model has eight entities. Membership links a user to a club with a role, so one person can belong to several clubs with different roles.

| Entity | Key fields | Relationships |
| --- | --- | --- |
| User | id, name, email, language (en or pt), created_at | Has many Memberships |
| Club | id, name, logo_url, timezone, created_by | Has many Memberships, Teams, Events, Venues |
| Membership | id, user_id, club_id, role (admin, coach, player), status (pending, active, removed) | Belongs to User and Club |
| Team | id, club_id, name, division (Div 2, Div 3, Div 4), gender (men, women) | Has many Memberships (via TeamMember) |
| Venue | id, club_id, name, address, map_url | Has many Events |
| Event | id, club_id, team_id, venue_id, type (match, training, social), title, starts_at, ends_at, capacity, rsvp_deadline, status (scheduled, cancelled) | Has many Rsvps and AttendanceRecords |
| Rsvp | id, event_id, user_id, status (going, not_going, maybe, waitlist), created_at | Belongs to Event and User |
| AttendanceRecord | id, event_id, user_id, status (present, absent, late), marked_by, marked_at | Belongs to Event and User |
| Invite | id, club_id, token, expires_at, revoked_at | Belongs to Club |

All times are stored in UTC and shown in the club's timezone. Deletions are soft (status or deleted_at) so attendance history is never lost.

## Tech stack

Recommended: one TypeScript codebase using Next.js, with Supabase as the backend and Vercel for hosting. This gives a web app that works on phones as a PWA, with a real database and login from day one, and no server to maintain. It fits a solo builder and keeps a native app possible later.

| Layer | Choice | Why |
| --- | --- | --- |
| Frontend | Next.js (React) with TypeScript | One codebase for web and mobile browsers; server components keep pages fast |
| UI | Tailwind CSS and shadcn/ui | Accessible components, fast to customise to our design |
| Backend and database | Supabase (Postgres, Auth, Storage, Realtime) | Postgres fits the relational model; Row Level Security enforces who sees which club |
| Realtime | Supabase Realtime | Live RSVP lists and waitlist promotion without refresh |
| Internationalisation | next-intl | English and Portuguese routing and message files from the start |
| Offline and install | Serwist (service worker) | Installable PWA with a basic offline shell |
| Email (later) | Resend | Invitations and reminders when we add email |
| Testing | Vitest (unit), Playwright (end to end) | Covers the four core flows and the permission rules |
| Hosting | Vercel (app) and Supabase (data) | Free tiers cover early use; both have easy preview environments |

**Alternatives considered**

- Firebase: fast to start, but the NoSQL model makes club-scoped permissions and reports harder to get right.
- Native first (Expo or React Native): better store presence, but doubles the work before we know the product is right. Revisit after v1.
- Self-hosted Postgres with a custom API: most control, most maintenance; not worth it for a solo v1.

The data model above maps directly to Postgres tables, with a Row Level Security policy per table that checks the user's Membership in the club.

## Internationalisation

Every user-facing string lives in a message file, never hard-coded. English ships first, Portuguese second, and both are in the first release. Adding a language later means adding one file.

- Language is chosen at sign-up and changed in settings. The choice is saved to the user profile and applies on every device.
- Dates, times, and numbers follow the chosen language and the club's timezone.
- Portuguese variant: Brazilian Portuguese (pt-BR) is the default, since it is the larger audience. European Portuguese (pt-PT) can be added later.
- Right-to-left support is out of scope for v1.
- Translation is done by the author, with a review pass before release.

## Non-functional requirements

These apply to every feature. They are checked before each release.

- **Privacy and data protection.** Data is stored in EU, UK, and Brazil regions. The app follows GDPR, UK GDPR, and Brazil's LGPD. Users can export and delete their data. The privacy policy is published before any outside user signs up.
- **Minors.** Some members are under 18. A minor's profile shows only name and club. Contact details are hidden from other members by default. Guardian access is designed in v1 but enabled in v2.
- **Security.** Passwords are hashed by Supabase Auth. Row Level Security is on for every table. Invite links are random, expiring tokens. All traffic uses HTTPS.
- **Access control.** A user sees only clubs they are an active member of. Only admins change roles. Coaches manage events and attendance for their teams.
- **Accessibility.** Target WCAG 2.2 AA: keyboard use, visible focus, sufficient contrast, labelled form fields, and screen-reader names for RSVP buttons.
- **Performance.** The main screens load in under 2 seconds on a mid-range phone over 4G. RSVP changes appear for other users within 3 seconds.
- **Reliability.** Uptime target of 99.5 percent for v1. Daily database backups with 7-day retention.
- **Testing.** The four core flows have end-to-end tests. Permission rules have unit tests. Both run before every release.
- **Observability.** Errors are logged with the user and club IDs (no personal data in messages).

## Roadmap

Seven phases, each ending at a gate. A phase does not start until the previous gate passes. Dates are proposals for a part-time solo build; adjust them to your hours.

1. **Spec (14 Oct to 31 Oct 2026).** Finish this document and settle the open questions. Gate: spec approved by you.
2. **Visual design (1 Nov to 21 Nov).** Wireframes, then high-fidelity screens for the four core flows, in English and Portuguese. Gate: the four flows are signed off as mockups.
3. **Foundation (22 Nov to 5 Dec).** Repository, Supabase schema and policies, sign-in, language switch, PWA shell, CI with tests. Gate: a user can sign up, log in, and switch language on a phone.
4. **Core build (6 Dec to 16 Jan 2027).** Clubs, roster, invites, events, RSVP with waitlist, attendance. Gate: all four core flows pass end-to-end tests.
5. **Internal pilot (19 Jan to 13 Feb 2027).** Your own club uses the app for four weeks. Gate: two weeks with no blocking bugs and attendance recorded for at least eight sessions.
6. **Polish and release (16 Feb to 6 Mar).** Portuguese review, accessibility audit, privacy policy, release checklist. Gate: release checklist passes and the app is live for invited clubs.
7. **After v1 (from March 2027).** Guardian access, email reminders, venue library, recurring events. Native apps are considered once v1 has active use.

The timeline will be drawn in the visual phase.

## Open questions and decisions

These need an answer before the visual phase starts (21 Nov at the latest, for the mockups).

- [x] Confirm the Portuguese variant: Brazilian (pt-BR, proposed) or European (pt-PT).
- [x] Confirm the success-criteria targets in Overview, or propose different ones.
- [x] Confirm that guardian access waits for v2, or whether your first club needs it in v1.
- [x] Confirm the PWA-first mobile approach, or require native store apps for v1.
- [x] Choose a product name. "Spike" belongs to another app, so a different name is needed before release.
- [x] Confirm the data region (EU or UK) and whether the club will be in the UK, the EU, or Brazil.
- [x] Confirm the first club's typical size and teams, so the roster design is sized correctly.

## Decisions (14 Oct 2026)

- Portuguese: Brazilian (pt-BR) is the default.
- Success criteria: the targets in Overview are confirmed.
- Guardian access moves to v2.
- Mobile: PWA first; native apps after v1.
- Product name: 5x1.
- Data regions: EU, UK, and Brazil. Brazil's LGPD applies alongside GDPR.
- First club: IVI (International Volleyball Ireland), with about six teams across men's and women's categories in the national league: Div 2, Div 3, and Div 4. A team therefore needs a division and a gender, and one club has many teams.

## Sources

Reference product reviewed for this spec (public pages only, as of 14 Oct 2026):

- [Spike homepage](https://www.volleyspike.app/)
- [Spike on Google Play](https://play.google.com/store/apps/details?id=app.volleyspike.mobile)
- [Spike on the App Store](https://apps.apple.com/app/id6792373783)
