<!--
Avannis PR template. Every PR closes exactly one tracked issue. Fill in the
number on the `Closes #` line below — that line is what links this PR to its
issue on project #4 and drives the board automation (Status moves + the
Pull Request field). No number = the card won't move and you'll be updating it
by hand. A bare `(#NNN)` in the title does NOT count — it must be a closing
keyword (Closes / Fixes / Resolves) in this body.
-->

Closes #

## What

<!-- The change in 1–2 sentences. What is different after this merges? -->

## How

<!-- Key implementation choices — anything a reviewer needs to follow the diff. -->

## Testing

<!-- How you verified this: commands run, what you checked on the integration branch. -->

## Smoke test

<!--
These run in candidate after this merges, and again in production. They prove
the ENVIRONMENT didn't change the behavior — not that the code is correct.
Correctness is established locally, before this PR ever goes live.

HOW MANY: count the seams, not the code. Ask "what could work on my machine
and still fail in candidate?" Each distinct answer is one box.

A seam is usually a new route, an endpoint called here for the first time, a
permission or role gate, a file upload or download, a migration, a queued job,
or anything talking to a per-environment service (Cognito, S3, a database).

A 2,000-line refactor behind one existing endpoint is one box. A 50-line change
adding a route, a gate and a CSV download is three. One box is a legitimate
answer. Five is the ceiling — if you need more, this PR is probably doing more
than one thing.

HOW TO WRITE THEM: so that someone who didn't write the code can run them
without reading the diff. Happy path, straight down the middle. No edge cases,
no per-field checks. For CRUD that is usually: create one and see it in the
list, edit it and confirm the change saved, delete it and confirm it's gone.

FORMAT: write each step as a markdown task-list line, dash-bracket-space. Only
those lines are picked up — prose here is dropped, and this section is
deliberately left empty rather than seeded with a blank checkbox, because an
empty box would be scraped onto the promotion PR as a meaningless unticked item
and would suppress the "no smoke steps were given" fallback.

These are copied onto the promotion PR when this merges to candidate, and
production stays blocked until every box is ticked.
-->

## Definition of Done

- [ ] Acceptance criteria on the linked issue are met
- [ ] Tests added/updated, CI green
- [ ] No new lint/type errors
- [ ] Docs updated (or n/a with reason)
- [ ] Merged to the integration branch (candidate / dev)
- [ ] Opened as a draft so automated review ran first

## Notes for reviewer

<!-- Optional: risk areas, things to look at closely, follow-ups deferred. -->
