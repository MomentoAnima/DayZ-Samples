# Terje medical rewrite + TUR Medical UI — working notes

Status: not committed. Notes location (Discussions / Projects / repo) still to be decided by the user.

## Session facts (verified 2026-09-30)
- This session only has MomentoAnima/DayZ-Samples (a Bohemia fork, HEAD da5e543). It has no Terje code, no medical UI, and no prior notes.
- The GitHub MCP tools here have no Discussions or Projects v2 access.
- Open questions to the user: where the Terje source is, where the UI code is, which repo is the target, what "scripting space" means, scope, and the Terje license.

## Requirements
### R1 — Auto-stow the held item before a menu treatment (#203, test 4, 2026-09-30)
Source: relayed by the peer session "Power System / Zone System" (session_013ouwvF2A7SYDL2mALaHbw6), quoting the test coordinator. It says the same text is posted on #203; I have NOT verified that (repo unknown, GitHub tools unavailable).
- User quote: "having it put away the thing for you would be great. As long as it's not going to be a situation where it ever drops anything."
- When a treatment starts from the menu while something is in hand, move the held item into inventory first.
- NEVER drop the item on the ground.
- If there's no inventory room, refuse the treatment and show the player a message.
- Treatment flow: passed test 4.
- Still open: approval of the menu wording.

### Implementation notes for R1 (hypothesis — no code seen yet)
- Find a free slot with the vanilla inventory API (e.g. `GetInventory().FindFreeLocationFor(item, FindInventoryLocationType.CARGO | ATTACHMENT, dst)`) BEFORE starting. If there's no location, refuse. Do not rely on any path that falls back to dropping (vanilla hand-swap / `PredictiveDropEntity`).
- Decide server-side (authoritative), and wait until the move completes before starting the treatment action.
- Guard the edge case where the stow succeeds but the treatment then fails to start (the item stays in inventory, which is acceptable; it must never be dropped).
