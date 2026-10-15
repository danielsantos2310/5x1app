# DESIGN — UI/UX Specification
Version 0.1 | Status: Design direction proposed; no mockups approved

## Design intent
Mobile-first sports utility, simple enough for a coach in a gym and a player responding while walking. Fast task completion over decorative complexity. Do not assume colors or logo are approved for 5x1; first IVI club branding can use its own crest/colors independently of platform brand.

## Information architecture
- Public: landing, login/signup/reset, invite redemption (English only; no language switch).
- Player: Home/upcoming, My Teams, Event Detail + RSVP, Notifications, Profile/settings.
- Coach: Player navigation plus Team Events, Attendance and Team Roster.
- Admin: Club Overview, Teams, Members/Approvals, Invitations, Events, Attendance, Club Settings.

## Screen inventory
| ID | Screen | Main elements |
| --- | --- | --- |
| UI-01 | Landing | headline, feature explanation, login, join |
| UI-02 | Auth | signup/login/reset, inline feedback |
| UI-03 | Club setup | club name, timezone, optional logo, steps |
| UI-04 | Member approvals | pending members; approve and assign one or more teams in one step (at least one team required); decline; team picker shows division and gender category |
| UI-05 | Teams | division, gender category, assigned member count |
| UI-06 | Team detail | roster, coaches, team events |
| UI-07 | Events list | upcoming/past, team filter, RSVP state |
| UI-08 | Event detail | time/venue, capacity, going/maybe/no, waitlist, participant list, admin in-app reminder (NOTE-01; no email or push in v1) |
| UI-09 | Create/edit event | title/type/start/end/venue/capacity/deadline, timezone warning |
| UI-10 | Attendance | going RSVPs only, large touch targets, one tap per person, running count, batch save with confirmation, read-only for cancelled events, correction with audit |
| UI-11 | Notifications | newest-first status changes, read state |
| UI-12 | Settings | language, accessible preferences, profile |
| UI-13 | Pending member | waiting-for-approval message, no team content, sign out only |

## Interaction rules
- Bottom navigation on phones for frequent player actions; admin tools accessible through clear menu.
- Primary RSVP buttons visible without scrolling on standard phone when practical; two taps or fewer from event card to confirmed response.
- Attendance status buttons large enough for touch, with explicit save confirmation and undo where possible.
- No permission-dependent control merely hidden: backend enforces access even if UI is manipulated.
- Loading skeleton/empty/error/retry/offline states for each feature.
- Consistent labels and translations, e.g. `Vou / Going`, `Talvez / Maybe`, `Não vou / Not going`, `Lista de espera / Waitlist`.

## Accessibility
Use semantic HTML, WCAG 2.2 AA as release target, keyboard traversal and focus visibility, screen-reader status announcements, 44x44 CSS-pixel recommended touch targets unless justified otherwise, appropriate contrast, no color-only statuses, reduced-motion support.

## Design deliverables before coding
1. Sitemap and site navigation model.
2. Low-fidelity wireframes of F01–F04 on phone, tablet and desktop.
3. Design tokens (colors, spacing, type, elevation, breakpoints) and club-level logo/avatar rules. Colours and light/dark tokens drafted in `docs/internal/design-tokens.md`; typeface (Inter), type scale, 4px spacing scale and breakpoints (390 / 768 / 1280px) approved 14 Oct 2026 and recorded there.
4. High-fidelity screens in English; pt-BR translation checked for text expansion, not shown as labels.
5. Clickable prototype and five-user usability test including player + coach + admin tasks.
6. Developer handoff: component variants, states, responsive measurements and accessible labels.

## Low-fidelity wireframes (v0.1, for review)
Source and captures live in `docs/internal/wireframes/`:
- `wireframes.html`: the four core flows, phone width, English only on screen. Dark mode is the default; a header button switches to light.
- `wireframes-dark.png` and `wireframes-light.png`: captures of the two themes at desktop width.

| Flow | Screen in the wireframes | Maps to |
| --- | --- | --- |
| F01 Club creation | Set up your club (4-step checklist) | UI-03 |
| F03 Events and RSVP | Player home with Vou / Talvez / Não vou | UI-07, UI-08 |
| F03 Events and RSVP | Admin event detail with waitlist, promote/hold, capacity warning | UI-08 |
| F04 Attendance | One-tap Present / Absent / Late, running count, save | UI-10 |

Status: layouts approved 15 Oct 2026 (tablet and desktop, `docs/internal/design/hifi/layouts-tablet-desktop-v0.1.html`). The interface palette and light/dark tokens are in `docs/internal/design-tokens.md`, with WCAG contrast checks. Tablet and desktop layouts are not yet drawn (deliverable 2 above is partly met).

## Design boards (14 Oct 2026, light and dark)
Two boards with five phone screens, a palette, typography, components, navigation and a club brand example. Superseded (15 Oct 2026) by the v0.2 board, `docs/internal/design/5x1-design-board-v0.2.html`. The older images `docs/internal/design/court-side-board-light.png` and `court-side-board-dark.png` showed Portuguese copy and a PT-BR/EN switch on the landing, which conflicted with the agreed rules; they are kept for the record only.

## High-fidelity screens (v0.1, for review)
Source: `docs/internal/design/hifi/hifi-player-admin-v0.1.html`. Covers UI-07/UI-08 entry (player home) and UI-04/admin overview, in English, light and dark, on the approved tokens (Inter, 4px spacing, 16px gutter, 44px minimum targets). Event detail (UI-08 admin) and attendance (UI-10) are next. Interactions are static in this version. Approval with team assignment (UI-04) and the pending waiting state (UI-13) are in `docs/internal/design/hifi/approvals-v0.1.html`: approval and team assignment are one step, approval needs at least one team, and the approve button names the team. Event detail (UI-08, admin, with the capacity-lowered warning) and attendance (UI-10, full-word Present / Absent / Late buttons, no bulk action) are in `docs/internal/design/hifi/eventatt-v0.2.html` (v0.1 is kept for the record). v0.2 adds a waitlist tab where each waitlisted RSVP has Promote, Hold and Remove, 44px targets, with each action audited. Hold keeps the person on the waitlist and skips them in automatic promotion until released (approved 15 Oct 2026). Decided 15 Oct 2026: promoting into a full event asks the admin to raise capacity first. Landing and sign-in (UI-01, UI-02, English only, no language switch) are in `docs/internal/design/hifi/landing-v0.1.html`.

## Platform brand identity (approved 14 Oct 2026)
The 5x1 brand is the geometric hexagon mark with the "5x1" wordmark, plus a standalone icon and app icon. Full rules are in `docs/internal/brand/5x1-brand.md`; the approved sheet is `docs/internal/brand/5x1-brand-identity-sheet.png`. Files:
- Primary logo: `5x1-logo-light.svg` (navy and green) and `5x1-logo-dark.svg` (off-white and green).
- Standalone icon: `5x1-icon-light.svg` and `5x1-icon-dark.svg`.
- App icon: `5x1-app-icon-dark.svg` (default, navy tile) and `5x1-app-icon-light.svg` (white tile), with PNG exports in `app-icon/` at 512, 192 and 180 px.

The platform logo sits on public and platform screens (landing, sign-in), centred on the landing page. The IVI crest stays on club pages only.

## Clickable prototype (15 Oct 2026)
A single-page prototype of the approved screens lives at `docs/internal/design/prototype/index.html` (see its `README.md`). It covers landing and sign-in, player home with Going / Maybe / Not going and automatic waitlist promotion, admin approval with team assignment, the event waitlist with promote, hold and remove, and attendance. It uses the approved tokens and brand, and runs in light, dark and system themes. It is for review and the usability test; it has no real data or sign-in.

Review pass (15 Oct 2026): player home shows the avatar beside the greeting; the team page shows the team name and logo; profiles take a photo and a position; admins have profiles and the More tab lists the club's admins; sign-in offers player / coach and admin; coaches can add and remove players on their own team, create events and take attendance (decision 40). Event creation requires title, date, time and venue, and defaults the RSVP deadline to 2 hours before the start. Team gender is not shown in the prototype.

## Sitemap and navigation (proposal, 14 Oct 2026)
Full site map, route names per screen and navigation by role are in `docs/internal/sitemap.md`. Two decisions are open there: coach navigation, and whether Teams sits in the admin Home tabs or the bottom bar.

## Brand (resolved)
The 5x1 brand mark, palette, typography and iconography are decided (see the platform brand section above). IVI remains the tenant and first launch club, with its own crest and colours on club pages; 5x1 owns the platform interface.
