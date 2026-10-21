# Usability test plan — prototype v0.2

Status: draft for owner approval. Closes the usability item in phase 1 (05-PLAN).

## Goal
Check that a player, a coach and an admin can finish their core tasks on a phone, without help, in the time the spec expects. Find the blocking friction before any code is written.

## Who
Five people, recruited from IVI, with at least one of each role. Ideally two players from different teams, one coach, and two admins (one of them the club's main admin). Consent and the test notice are sent first. No one under 18 (pilot rule, decision 5). Sessions: 45 minutes, on the participant's own phone or a test phone, in person or on a call.

## What we test
The prototype (`docs/internal/design/prototype/index.html`, published link as the owner shares it). It uses sample data and fake sign-in. Tell participants that nothing is saved and it is not the real app.

## Tasks (read aloud, one at a time, no hints)
| # | Role | Task | Success |
| --- | --- | --- | --- |
| 1 | Player | Create an account and finish the profile as a player | Reaches the waiting screen without help |
| 2 | Player | Say whether you are going to Thursday's training, then change to not going | Both answers shown correctly |
| 3 | Player | Find out what happens if you answer going to a full event | Understands waitlist position |
| 4 | Player | Change your position and save it | Sees the save confirmation |
| 5 | Player | Find your Settings and switch to dark theme | Finds it under Profile |
| 6 | Coach | Add a club member to your team's roster | Member appears in the roster |
| 7 | Coach | Create a training event for your team | Event appears in the list |
| 8 | Coach | Take attendance for the last session, marking one person late | Saves; late shows correctly |
| 9 | Admin | Approve a pending member and assign two teams in one step | Member shows the right teams |
| 10 | Admin | Promote a waitlisted person to a full event | Asked to raise capacity first; understands why |
| 11 | Admin | Open a member's details (admins only) | Finds the emergency contact |

## Measures
- Task success (completed without help / with help / failed), per task.
- Time on task, for tasks 1, 2, 7, 8, 9 (compare against the 5-minute goal in the PRD).
- Number of wrong taps before the right one.
- Verbal confusion, recorded as quotes.
- System Usability Scale (10 questions), after all tasks.

## Pass criteria (from the spec)
- Each core task succeeds for at least four of five people.
- No task takes more than 2 minutes for a first-time user.
- No accessibility blocker found by the participant using their own text size.
- Any finding that stops a core task is fixed before phase 2 starts; other findings go on the backlog.

## Output
A one-page findings note: task table with results, top five issues with severity, quotes, and the changes made. Saved in `docs/internal/usability-results-<date>.md`. Findings feed the prototype's next version, then the developer handoff.

## Ethics and data
Participants are told they can stop at any time. Sessions are not recorded unless the participant agrees. Notes use participant codes (P1 to P5), not names. Notes are kept in the internal drive and deleted after the phase 2 gate.
