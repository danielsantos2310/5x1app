# 5x1 — Project status (read this first)

Last updated: 21 Oct 2026. Update this file at the end of every working session so the next session does not need to re-read the whole project.

## What 5x1 is
Volleyball club management app. First club: IVI (International Volleyball Ireland). Bilingual target (English, pt-BR planned). Next.js + TypeScript, Supabase (Postgres, Auth, Storage, Realtime, RLS), Vercel, PWA. Hosting region Ireland (eu-west-1). Internal repo, branch `main`.

## Working rules agreed with the owner
- Linear steps: spec, then visual design, then code. Owner approves each step.
- Owner said: no need to ask permission for routine work; commit and push to `main` as each step is done.
- COURT-SIDE is not used. Decision records may mention that it is not used; nothing else.
- Pages (GitHub) stays as it is.

## Where the project is
- Phase 0 (spec): done. Nine spec docs in `docs/spec/`, decisions register in `docs/product-spec.md` (46 decisions, latest 45 new-member sign-up, 46 profile save and settings).
- Phase 1 (UX/UI): mostly done. Approved: design board, design tokens, sitemap, brand identity, tablet and desktop layouts, event detail v0.2. Done but review pending: hi-fi player/admin, approvals, landing/sign-in.
- Clickable prototype: `docs/internal/design/prototype/index.html` (README beside it). At v0.2, all changes pushed; published artifact 7wV9A8uJ2aMR4wEKkPH1zk (private). Covers sign-in (one player/coach entry, one admin entry), role chosen on Complete your profile, create account, profile (photo circle, position for players, staged details with Save changes), Settings row (theme, language), admin tabs Home/Events/Members/Club/Profile, club sub-screens, member details (admin only), waiting for approval.
- Phase 2 (foundation): not started. Planned start 22 Nov 2026.
- Code: none yet. Only the prototype exists.

## Open items
- Five-user usability test plan: not started (needed to close phase 1).
- Developer handoff: not started.
- Legal review: under-18 gate, data controller role.
- IVI official colour values.
- Designer's vector source for the logo (current logo is traced).
- Android and favicon icon sets.
- Missing doctype on older hi-fi HTML files.
- `docs/spec/05-PLAN.md` design table is stale and has duplicate rows for usability test and handoff.
- Prototype test scripts (Playwright, in the working session, not in repo) need updating to the new sign-in flow.

## Decisions most likely to matter next
- 40 coach permissions (own teams only; approvals stay with admins)
- 41 profiles and photos (multiple admins; position for players only)
- 42 team gender removed (gender only on social events)
- 43 player details (height, weight, address, emergency contact; admin-only visibility; planned for later)
- 44 admin navigation (Club tab; sub-screens return to Club)
- 45 new member sign-up and setup (role chosen on profile step; waiting for approval)
- 46 profile save and settings (Save changes; Settings holds theme and language)

## How to verify the prototype
Open `docs/internal/design/prototype/index.html` in a browser. For automated checks, use headless Chromium at `/opt/pw-browsers/chromium` with Playwright (Python). Click via `document.querySelector(...).click()` for nav buttons that Playwright reports as hidden.

## Next step proposed
1. Owner reviews prototype v0.2 flows.
2. Update `05-PLAN.md` design table and remove duplicate rows.
3. Draft the five-user usability test plan to close phase 1.
