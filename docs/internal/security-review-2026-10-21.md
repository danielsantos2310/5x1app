# Security and code review — prototype (21 Oct 2026)

Scope: `docs/internal/design/prototype/index.html`, the only code in the repo. There is no backend yet, so there is nothing to attack server-side. The review was targeted, not a full multi-agent scan, to keep it cheap. Run a full scan before phase 2 code lands.

## Checked
- No external scripts, stylesheets, or links to third-party hosts.
- No `eval`, `document.write`, or inline event handlers.
- User-entered values (event title, venue, date, time, capacity, names, emails) are passed through `escHtml` before entering markup.

## Found and fixed
- Event title and venue, and the event heading, were inserted into markup unescaped. A title such as `<img src=x onerror=...>` ran script. Fixed (commit "Escape user-entered values"). Verified with a test payload: it now shows as text and no script runs.

## Carried into the build (not prototype issues)
- Real authentication, row-level security on every table, and club isolation tests (see the plan's first risk).
- Invite tokens stored as hashes; expiry and revocation (CLUB-02).
- Player details visible only to admins, enforced in the database, not only in the UI (decision 43).
- Under-18 sign-up closed until the legal review is done (decision 45).
- Output encoding in the real app: use the framework's escaping, avoid raw HTML insertion.
