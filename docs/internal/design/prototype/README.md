# 5x1 Clickable Prototype (v0.2)

Status: review pass 15 Oct 2026, for review. Internal, not published.
File: `index.html` (one page, no build step). Open it in a browser, or use the "Jump to" list to go straight to a screen.

## What it covers
- **Landing and sign-in** (UI-01, UI-02): English only, no language switch. Sign-in offers "Continue as player / coach" and "Continue as admin". There is no real authentication yet.
- **Player**: home with the avatar beside "Hi, João!", the next training card and Going / Maybe / Not going (UI-07, UI-08), events list, team page with the team name and logo, profile with photo and position.
- **Coach**: the player screens plus coach tools on home (manage roster, create event, take attendance, waitlist). On the team page a coach can add existing club members and remove players, with confirmation.
- **Admin**: club overview with the set-up checklist (UI-03), approvals with one-step approval and team assignment (UI-04), events list, event detail with Confirmed, Waitlist and Maybe tabs (UI-08), attendance (UI-10), create event, profile with photo, and More with the list of club admins.

## Rules the prototype applies (from the spec)
- **Maybe** does not hold a place and does not count against capacity (RSVP-01, decision 14 Oct).
- **Going** takes a free place; if the event is full the player joins the waitlist and sees their position (RSVP-02).
- **Automatic promotion**: when a going player leaves, the earliest waitlisted player moves up, skipping anyone on hold. The message names everyone who moved up.
- **Hold** keeps a waitlisted person on the waitlist and skips them in automatic promotion until released (decision 38).
- **Promote into a full event**: the admin is asked to raise capacity first (decision 39).
- **Coach permissions** apply to the coach's own team only (decision 40). Approvals stay with admins.
- **Admin profiles**: several admins, each with a profile (decision 41). Only players have a position.
- **Player details** (planned, decision 43): height, weight, address and emergency contact are on the player profile. Only club admins see them, through Club › Club members. The prototype has no real access control.
- **New member** (decision 45): sign-in has one player/coach entry and one admin entry. The player/coach entry goes to Complete your profile, where the person enters their name and chooses Player or Coach. Create account goes to the same step. Then Waiting for approval. Sign-in uses no real account data.
- **Profile photo**: the photo circle is the control. With no photo, tapping the circle adds one. With a photo, a small camera badge on the corner opens Upload new photo, Remove photo or Cancel.
- **Admin navigation** (decision 44): Home, Events, Members, Club, Profile. Team roster, club members, club settings and invitations open from Club and go back to Club.
- **Team labels**: division and team name only, e.g. "Div 4 · Ravens". Team gender is removed (decision 42); gender still applies to social events (decision 19).
- **Approval** needs at least one team; the approve button stays disabled until one is picked (decision 32).
- **Attendance** marks only confirmed players, one tap per person, with a running count. Save is enabled once someone is marked. No bulk "mark all".
- **Create event**: title, date, time and venue are required. Capacity is optional. The RSVP deadline defaults to 2 hours before the start.

## Data used (prototype only)
- Capacity 10. Confirmed 9 (including João Pereira, the signed-in player). Waitlist: Lucas Martins, Ethan Ferreira. Maybe: 3 players.
- Pending approvals: Sofia Ribeiro, Tomás Moreira. Club members not on the team: Otávio Braga, Nando Reis.
- Teams: the six teams from the team list. The signed-in player's team is "Div 3 Dragons". This is a placeholder until team assignments are confirmed.
- Admins: Daniel Santos, Ana Costa.

## Known gaps (not in this version)
- Photo and team logo are stored in the browser for the session only. Nothing is uploaded.
- Invite an admin, club settings, notifications and Settings are placeholders.
- Edit event is not in this version.
- Nothing is saved between visits. Data resets on reload.
- Not yet tested with users (see the usability test in the plan).
