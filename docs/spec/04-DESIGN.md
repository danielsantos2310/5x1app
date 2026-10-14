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
| UI-04 | Member approvals | pending members, approve/decline |
| UI-05 | Teams | division, gender category, assigned member count |
| UI-06 | Team detail | roster, coaches, team events |
| UI-07 | Events list | upcoming/past, team filter, RSVP state |
| UI-08 | Event detail | time/venue, capacity, going/maybe/no, waitlist, participant list |
| UI-09 | Create/edit event | title/type/start/end/venue/capacity/deadline, timezone warning |
| UI-10 | Attendance | going RSVPs only, large touch targets, one tap per person, running count, batch save with confirmation, read-only for cancelled events, correction with audit |
| UI-11 | Notifications | newest-first status changes, read state |
| UI-12 | Settings | language, accessible preferences, profile |

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
3. Design tokens (colors, spacing, type, elevation, breakpoints) and club-level logo/avatar rules. Colours and light/dark tokens drafted in `docs/internal/design-tokens.md`; spacing, type scale and breakpoints still open.
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

Status: awaiting approval. The interface palette and light/dark tokens are in `docs/internal/design-tokens.md`, with WCAG contrast checks. Tablet and desktop layouts are not yet drawn (deliverable 2 above is partly met).

## Design boards (14 Oct 2026, light and dark)
Two boards with five phone screens, a palette, typography, components, navigation and a club brand example. Images: `docs/internal/design/court-side-board-light.png` and `court-side-board-dark.png`. They show Portuguese copy and a PT-BR/EN switch on the landing, which conflict with the agreed rules; the conflicts are listed in `design-tokens.md` for decision.

## Brand decision needed
Define 5x1 independent brand mark (not IVI logo), visual palette, typography, iconography. IVI is the tenant/first launch club; 5x1 owns platform-wide interface.
