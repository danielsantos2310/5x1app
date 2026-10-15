# 5x1 Clickable Prototype (v0.1)

Status: built 15 Oct 2026, for review. Internal, not published.
File: `index.html` (one page, no build step). Open it in a browser, or use the "Jump to" list to go straight to a screen.

## What it covers
- **Landing and sign-in** (UI-01, UI-02): English only, no language switch. Sign-in has a prototype-only role choice (player or admin); there is no real authentication yet.
- **Player**: home with the next training card and Going / Maybe / Not going (UI-07, UI-08), events list, team roster, profile.
- **Admin**: club overview with the set-up checklist (UI-03), approvals with one-step approval and team assignment (UI-04), events list, event detail with Confirmed, Waitlist and Maybe tabs (UI-08), attendance (UI-10), more.

## Rules the prototype applies (from the spec)
- **Maybe** does not hold a place and does not count against capacity (RSVP-01, decision 14 Oct).
- **Going** takes a free place; if the event is full the player joins the waitlist and sees their position (RSVP-02).
- **Automatic promotion**: when a going player leaves, the earliest waitlisted player moves up, skipping anyone on hold. The message names everyone who moved up.
- **Hold** keeps a waitlisted person on the waitlist and skips them in automatic promotion until released (decision 38).
- **Promote into a full event**: the admin is asked to raise capacity first (decision 39). The dialog offers the raise and promote, or cancel.
- **Remove** asks for confirmation on the same row before it happens.
- **Approval** needs at least one team; the approve button stays disabled until one is picked (decision 32).
- **Attendance** marks only confirmed players, one tap per person, with a running count. Save is enabled once someone is marked. No bulk "mark all".
- **Cancelled events** are read-only, but no cancelled event is in the prototype data.

## Data used (prototype only)
- Capacity 10. Confirmed 9 (including João Pereira, the signed-in player). Waitlist: Lucas Martins, Ethan Ferreira. Maybe: 3 players.
- Pending approvals: Sofia Ribeiro, Tomás Moreira.
- Teams: the six teams from the team list (Div 2 Vixens and Dinosaurs, Div 3 Lynnx and Dragons, Div 4 Regional League Phoenix and Ravens).
- The player's own team is shown as "Div 3 Dragons". This is a placeholder until the team assignments are confirmed.

## Known gaps (not in this version)
- Team gender (men or women) is not shown, because it is not yet confirmed per team. The high-fidelity screens still show "Ravens · Men · Div 3", which does not match the team list. Needs confirming.
- Edit event, invitations, club settings, notifications, team and profile details, and Settings are placeholders.
- Nothing is saved between visits. Data resets on reload.
- Not yet tested with users (see the usability test in the plan).
