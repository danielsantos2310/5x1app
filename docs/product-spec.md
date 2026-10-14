# 5x1 — Product Spec & Roadmap (summary)

2026-10-14 · Daniel Santos · Status: decisions recorded, spec in review

This is the summary. The full document package lives in [`docs/spec/`](spec/00-README.md): PRD, TRD, flows, design, plan, backend, testing, and security and privacy. Where the two disagree, the full package is the reference and this summary should be corrected to match. These documents are internal and are not published.

## Overview and goals

5x1 is a free, bilingual web and mobile app that lets a volleyball club run its members, teams, events, RSVPs and attendance in one place. v1 launches with International Volleyball Ireland (IVI). Every table and permission is built for several clubs from day one, so a second club can join without a database redesign.

v1 goals:

- Run IVI end to end: teams, roster, events, RSVP and attendance.
- Work in the browser and on phones from one codebase (installable PWA).
- Ship in pt-BR and English at launch. Portuguese is the default.

Success targets (confirmed):

- An admin can create a club, invite members and schedule an event in under 5 minutes.
- A member can RSVP in two taps or fewer, not counting sign-in.
- An admin can record attendance for a fixed roster in under 2 minutes.

Timer start and end, device profile, sample size and the 12, 16 and 20-player roster tests are defined in the [testing document](spec/07-TESTING.md).

## Pilot club: IVI

IVI runs six teams across two categories:

| Division | Men's team | Women's team |
| --- | --- | --- |
| Div 2 | Dinosaurs | Vixens |
| Div 3 | Dragons | Lynnx |
| Div 4 (Regional League) | Phoenix | Ravens |

Team names and divisions are configuration data, not hard-coded. Other clubs can use their own divisions. Confirm that "Regional League" is the name of the Div 4 league, not a separate division.

Pilot players are all adults. No one under 18 is in the pilot.

## Users and roles

Roles are assigned per club and per team, not as one global role field:

- **Club admin:** configures the club and teams, approves members, assigns roles, creates and cancels events, reviews attendance.
- **Coach:** sees and manages only the teams they are assigned to: roster, events and attendance.
- **Player:** sees their teams, RSVPs to events they are invited to, and views their attendance where allowed.
- **Guardian:** not in v1. Guardian access and linked minor profiles come in v2.

One person can hold several roles: a coach can also play, and an admin can also play on a team. Team access is checked on the server, never by hiding buttons.

## Scope for v1

**In scope**

- Accounts: sign up, log in, reset password, pt-BR and English UI.
- Club: create a club with a name, logo and timezone; invite by expiring or revocable link; approve join requests.
- Teams: create teams with a division and a gender category; assign members to one or more teams.
- Events: title, type (match, training, social), start and end, venue as text with an optional map link, optional capacity (blank means no limit), and RSVP deadline (pre-filled to 2 hours before the start). Events can be club-wide or for one team. Social events can also be set to `mixed`.
- RSVP: members mark going, not going or maybe, and see the live list of who is going. Maybe does not hold a place or count against capacity. Members can opt in to the waitlist when an event is full. Promotion from the waitlist is automatic and in order; admins can promote, hold or remove any waitlisted RSVP at any time (for example for payment or team needs). Lowering capacity below the number already going keeps those people going, sends new RSVPs to the waitlist, and shows the admin a warning. After the deadline, members cannot change their RSVP; admins can.
- Attendance: present, absent or late, recorded by an admin or the team's coach. Attendance can only be recorded for people with a going RSVP. Walk-ins are not handled in the app. Changes are audited.
- Basic in-app notifications. Email is optional.
- Installable PWA.

**Visibility of team events:** the setting is built so admins can limit a team event to that team, but it is switched off for the internal pilot. Until it is switched on, any club member can see any event in their club.

**Out of scope for v1:** payments, team chat, social feed and polls, drills and training plans, player evaluations, guardian and family features, leagues, match scoring and statistics, native app store releases, and monetisation.

**Deferred to v2 or later:** guardian access, email reminders, a venue library with photos and maps, recurring events, registration forms, attendance streaks, and languages beyond pt-BR and English.

## Core user flows

1. **Create a club.** Admin signs up, names the club and gets an invite link.
2. **Join a club.** Player opens the invite link, signs up or logs in, and joins or sends a request for approval.
3. **Schedule and RSVP.** Admin or coach creates an event. Members tap going, not going or maybe. A full event puts the next person on the waitlist, who is promoted when a place frees up.
4. **Record attendance.** The coach or admin opens the event, sees the RSVP list and marks each person present, absent or late.

## Functional requirements

Each requirement has an ID and is traced to flows, screens, tables and tests in the [PRD](spec/01-PRD.md) and [testing document](spec/07-TESTING.md).

| ID | Module | Requirement |
| --- | --- | --- |
| AUTH-01 | Accounts | Sign up, log in and reset password by email, without revealing which emails have accounts. |
| I18N-01 | Language | pt-BR default; full English; the choice is saved to the profile. |
| CLUB-01 | Club | Create a club with a name, logo and timezone; the creator becomes admin. |
| CLUB-02 | Club | Invite links can be expired or revoked; join requests need admin approval. |
| TEAM-01 | Teams | Several teams per club, each with a division and a gender category. |
| ROST-01 | Roster | Assign members to one or more teams; duplicates are rejected. |
| ROLE-01 | Roles | Several roles per person; admin and coach rights enforced on the server. |
| EVT-01 | Events | Create, edit and cancel events for the club or one team. Capacity is optional; the RSVP deadline defaults to 2 hours before the start. Social events may be `mixed`. |
| RSVP-01 | RSVP | One active status per person per event; can be changed until the deadline. |
| RSVP-02 | RSVP | Never over capacity, even with simultaneous responses. Promotion is automatic and in order; admins can promote, hold or remove waitlisted RSVPs, with audit. Lowering capacity keeps current going RSVPs and warns the admin. |
| ATT-01 | Attendance | Present, absent or late; only authorised admins and coaches; changes audited. |
| NOTE-01 | Notifications | In-app notices for new, changed or cancelled events. |
| PWA-01 | Platform | Installable on phones. An offline shell does not mean offline RSVPs. |
| ACCESS-01 | Accessibility | WCAG 2.2 AA. |

## Data model

The model is in [backend design](spec/06-BACKEND.md). Main changes from the first draft:

- **Club memberships** and **club role assignments** are separate tables, so one person can hold several roles.
- **Team memberships** link people to teams with a membership type (player or coach).
- **Teams** have `division` (free text, not a fixed list) and `gender_category` (men or women). There is no season field in v1.
- **Events** can have a `team_id`; when it is empty, the event is club-wide.
- **RSVPs** and **attendance** are unique per person per event, and cannot reference another club's data.
- Every tenant table carries `club_id`, and constraints stop a row from pointing at another club's data.

## Tech stack

One TypeScript codebase: Next.js (App Router) with React and Tailwind CSS, shadcn/ui components, next-intl for pt-BR and English, and a service worker for the PWA. Supabase provides Postgres with Row Level Security, Auth, Storage and Realtime. The app is hosted on Vercel. Tests use Vitest and Playwright.

## Hosting and data protection

**Launch region: EU, Ireland (`eu-west-1`).** Each Supabase project has one primary region, so the database can't be in Ireland, London and São Paulo at once. UK (London, `eu-west-2`) and Brazil (São Paulo, `sa-east-1`) will need their own projects and a transfer review before any UK or Brazilian club joins.

For now, the basics of GDPR apply: a privacy notice, data export and deletion requests, no unnecessary personal data in logs, and processors listed. The controller question for IVI's data (club or platform) is deferred to legal review.

**Minors:** the pilot has no one under 18. Sign-up for under-18s stays restricted until a legal review is complete.

## Internationalisation

Every user-facing string lives in a message file. pt-BR is the default. For a first visit, a saved language preference wins; otherwise pt-BR is used, and the browser language is not used silently. English is a complete, first-class language from launch and is always one tap away. Dates and times follow the club's timezone and the chosen language.

## Non-functional requirements

- **Privacy:** GDPR basics now; UK GDPR and LGPD in the expansion review. Export and deletion requests supported.
- **Security:** Row Level Security on every table, deny by default. Invite tokens stored as hashes. Service keys never reach the browser. All traffic over HTTPS.
- **Performance:** main screens under 2 seconds on a mid-range phone over 4G; RSVP changes reach other users within 3 seconds.
- **Reliability:** 99.5% availability target; daily backups with 7-day retention, and restores tested.
- **Accessibility:** WCAG 2.2 AA, tested by keyboard and screen reader.
- **Testing:** the four core flows and the permission rules are tested before every release.

## Roadmap

Seven phases, each ending at a gate. Dates are proposals for a part-time build.

1. **Spec (14 Oct to 31 Oct 2026).** Finish the spec package. Gate: spec approved.
2. **Visual design (1 Nov to 21 Nov).** Wireframes and bilingual screens for the four flows. Gate: flows signed off.
3. **Foundation (22 Nov to 5 Dec).** Repository, CI, schema and Row Level Security, sign-in, language switch, PWA shell. Gate: sign-up, login and language switch work on a phone.
4. **Core build (6 Dec to 16 Jan 2027).** Clubs, teams, roles, invites, events, RSVP with waitlist, attendance. Gate: four core flows pass end-to-end tests.
5. **Internal pilot (19 Jan to 13 Feb 2027).** Our own club uses it for four weeks. Gate: two weeks without blocking bugs; attendance recorded for at least eight sessions.
6. **Polish and release (16 Feb to 6 Mar).** Portuguese review, accessibility audit, privacy notice, release checklist. Gate: checklist passes; invite-only release.
7. **After v1 (from March 2027).** Guardian access, email reminders, venues, recurring events; native apps considered after real use.

## Decisions (14 Oct 2026)

| # | Question | Decision |
| --- | --- | --- |
| 1 | First hosting region | Ireland (`eu-west-1`) |
| 2 | Do `maybe` RSVPs count against capacity? | No |
| 3 | Can non-team members see a team's events? | Not for now. Build the setting, leave it off for the internal pilot |
| 4 | Can an admin record attendance for someone who did not RSVP? | No. Walk-ins are not handled in the app |
| 5 | Any players under 18 in the pilot? | No |
| 6 | Brand for the pilot | IVI's colour palette |
| 7 | IVI teams and divisions | See the pilot club table above |
| 8 | Data controller for IVI's data | Deferred to legal review; basic GDPR now |
| 9 | Where the spec files live | `docs/spec/`, internal only; not published (Pages removed) |
| 10 | Season field on teams | Not in v1 |
| 11 | Gender categories | Men and women for teams; `mixed` only for social events |
| 12 | Coach visibility | Own assigned teams only: roster, events and attendance |
| 13 | Admin also a player | Yes |
| 14 | Maybe RSVPs and capacity | Maybe does not hold a place or count against capacity |
| 15 | Capacity field | Optional; blank means no limit |
| 16 | RSVP deadline | Required; pre-filled to 2 hours before the start |
| 17 | Waitlist promotion | Automatic and in order, with admin override (promote, hold or remove) |
| 18 | Lowering capacity below current going | Existing going RSVPs stay; new ones go to the waitlist; admin sees a warning |
| 19 | Social events gender | Optional; can be set to `mixed`, default open |
| — | Product name | 5x1 |
| — | Portuguese default | pt-BR |
| — | Success targets | Confirmed, with definitions in the testing document |
| — | Guardian access | v2 |
| — | Platform | Responsive web and PWA first; native apps after v1 |
| — | Data regions | Corrected: Ireland at launch; UK and Brazil later, each in its own project after review |

## Open items

- [ ] Confirm IVI's official colour values. Sampled values are in [`docs/internal/ivi-brand.md`](internal/ivi-brand.md).
- [ ] Confirm that "Regional League" is the name of Div 4.
- [ ] Data controller for IVI's data, with legal input.
- [ ] Brand for 5x1 itself, separate from IVI, before public launch.
- [ ] Legal review before any under-18 sign-up.

## Sources

Reference product reviewed for this spec (public pages only, as of 14 Oct 2026):

- [Spike homepage](https://www.volleyspike.app/)
- [Spike on Google Play](https://play.google.com/store/apps/details?id=app.volleyspike.mobile)
- [Spike on the App Store](https://apps.apple.com/app/id6792373783)

Hosting regions: [Supabase regions](https://supabase.com/docs/guides/platform/regions).
