# Developer handoff — 5x1 prototype v0.2

Status: draft. Source of truth for the look is `docs/internal/design-tokens.md` and the prototype (`docs/internal/design/prototype/index.html`). Where they differ, the tokens win and the prototype needs updating.

## Breakpoints and layout
- Phone 390 px (design width 360 px in the prototype frame); tablet 768 px; desktop 1280 px (side rail, approved 15 Oct 2026).
- 16 px side gutter; no horizontal page scroll at any width.
- 4 px spacing scale.

## Colour and type
- Use the tokens from `design-tokens.md` for light and dark. Never hard-code a colour in a component.
- Text on green, orange or flame is always navy (`#021C37`).
- Inter typeface. Body 16 px, minimum 14 px for helper text.

## Touch and focus
- Every tap target at least 44 px high and wide. Attendance buttons are full-word labels (Present, Absent, Late).
- Focus ring: 3 px green outline, 2 px offset (prototype `:focus-visible`).

## Components (state list each one must implement)
| Component | States | Accessible name / behaviour |
| --- | --- | --- |
| Button (primary, outline, danger) | default, pressed, disabled, loading | Visible text is the name. Disabled buttons still have a label. |
| RSVP choice (Going, Maybe, Not going) | default, selected, disabled while saving | `aria-pressed` on each choice; status text announced in `role="status"`. |
| Attendance segmented control | none, present, absent, late | Each option is a `aria-pressed` button, labelled "Status for <name>". |
| Status chip | going, waitlist #n, maybe, no answer, confirmed | Text and icon, never colour alone. |
| Avatar / photo button | no photo (initials, "Tap to add a photo"), photo, photo with corner badge | Label "Add profile photo" or "Change or remove profile photo". |
| Dialog | photo menu, raise capacity | `role="dialog"`, `aria-modal`, focus moves to first button, Escape closes. |
| Form field | default, focus, error, disabled | Label always visible; error text in `role="alert"`. |
| Toggle (Repeat, v2) | off, on | Native checkbox with a visible label; options hide when off. |
| Navbar | 4 tabs (player, coach), 5 tabs (admin) | Current tab has `aria-current="page"`. Tab count set by content, not fixed. |

## Behaviour rules
- Saving a profile: edits stay pending until Save changes. Leaving without saving discards them. Photo changes apply at once.
- Waitlist: automatic promotion skips held entries. Maybe RSVPs never hold a place.
- Admin-only data (player details, emergency contact) must not be sent to members or coaches, even if hidden in the UI.
- Sign-in: one entry for players and coaches; the role is chosen on Complete your profile.

## Security notes for the build
- Escape every user-entered value. The prototype's `escHtml` is the pattern to copy. Do not insert raw HTML.
- Follow `docs/internal/security-checklist.md`. Phase 2 closes its partial and missing items.

## Not in the prototype (build later)
Real sign-in and password reset, RLS, server validation, uploads, Portuguese text, notifications, Android and favicon icons, the official IVI colours, and the designer's vector logo.

## Acceptance
Every screen in the prototype is matched in the build. Automated tests cover each flow in `docs/spec/07-TESTING.md`. Accessibility check passes WCAG 2.2 AA on each core screen.
