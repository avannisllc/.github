# How We Work: Engineering Workflow

*Version 1 — expect this to change. If something here feels wrong, dumb, or unnecessary, tell me. This document is co-owned.*

---

## Why this exists

We're a small team moving across a lot of code. Our biggest risk isn't writing bad code—it's wasting cycles on ambiguity: building the wrong thing, building it too big to review, or finding out late that it wasn't what was needed. This workflow exists to kill ambiguity at the cheapest possible moment, push structuring work onto tools and lighter-weight humans, and reserve the rare slots of focused engineering time for actual engineering.

The intent is **less process, not more**—but the process we do have, we follow consistently.

---

## How work reaches you

You don't see raw customer requests, vague Slack messages, or half-baked ideas. Here's what happens before an Issue lands in your queue:

1. Customers and internal stakeholders open cases in Salesforce.
2. Support triages those cases. Most resolve there—how-tos, config questions, account issues, things that don't require code changes.
3. When support decides a case needs engineering, they flag it and write a short "customer need" summary (what the customer wants, what they're doing today, what's blocking them).
4. Claude enriches the escalated case with a draft title, summary, proposed repo, and priority suggestion.
5. The CTO reviews the enriched case and decides: kill, send back for clarification, or approve.
6. Approved cases automatically become GitHub Issues using our template, assigned to a dev, with the originating case linked.

**By the time you see an Issue, it's been filtered through three layers of structuring.** It should have a clear why, scope, acceptance criteria, and DoD. If it doesn't, push back—the system is failing upstream and we need to know.

---

## Once an Issue is assigned to you

### 1. Pre-flight before any code

For anything bigger than trivial (rough rule: anything over 2 story points), post a pre-flight comment on the Issue **before you start coding**:

- **Approach** — 2–3 sentences on the technical direction
- **Proposed breakdown** — if the work needs more than one PR, list the child Issues you'd create, each sized to one focused session and under 1000 lines of diff (not including tests or documentation). Sequence them, and name the first shippable slice.
- **Test plan** — behavior-level. What you'll verify, what you're *not* verifying (and why). Doesn't need to be exhaustive; a few bullets is fine.
- **Open questions** — anything ambiguous in the Issue that needs clarification before you commit to an approach
- **Risks** — anything that could expand scope or break things unexpectedly

You can draft this with Claude Code. Hand it the Issue and ask for the breakdown, test plan, and risks—then refine the draft and post it. Wait for a 👍 before writing code.

For trivial work (typo, small fix, obvious one-PR change), a one-line comment is fine. **Pre-flight scales with ticket size.**

**Why this matters:** the cheapest moment to catch a misunderstanding is before code exists. A 5-minute pre-flight prevents hours of rework. It's also where decomposition happens—so reviews stay reviewable and PRs stay small.

### 2. PR size cap: 1000 lines of meaningful diff

Hard rule. PRs over this will be auto-rejected by a bot with a comment asking you to split.

- Generated files, lockfiles, and auto-generated migrations don't count toward the limit.
- If you're using Claude Code, splitting a branch into a stack of dependent PRs is something it does well. Ask it to split your branch into reviewable units in dependency order.
- If a PR genuinely can't be split (rare), flag it in the description with a one-paragraph explanation. A reviewer will decide whether to accept.

**Why this matters:** large PRs get worse reviews, not better ones. A 10K-line PR is functionally unreviewable—it gets rubber-stamped or it eats a day of someone's life. Small PRs catch real issues, ship faster, and don't pile up review debt.

### 3. Branching and PR conventions

Mechanical, cheap to follow, removes friction:

- **Branches**: `feature/123-short-description` or `bug/124-short-description`
- **PR titles**: `[#123] Short description` — keep it scannable
- **PR description must include**:
  - `Closes #123` (so the Issue auto-closes on merge and status flows back to the Salesforce case)
  - What changed (2–3 bullets)
  - How to verify (steps the reviewer can run)
  - DoD checklist copied from the Issue, with current state

### 4. Self-review before requesting human review

Before tagging a reviewer, re-read your own diff as if you didn't write it. Add inline comments explaining non-obvious choices. Tick the DoD boxes you can verify yourself. Flag anything you're unsure about with an inline comment so the reviewer focuses there.

This catches 20–30% of what a reviewer would otherwise comment on and makes review dramatically faster.

### 5. Automated Claude review runs first

Every PR gets a first-pass review from Claude via GitHub Action. It checks DoD completeness, missing tests, obvious bugs, security smells, unclear naming, and edge cases.

**Address Claude's feedback before requesting a human reviewer.** Don't make the human re-flag what the bot already caught. You're not required to take every suggestion, but if you skip one, leave a brief comment explaining why.

The goal: by the time a human looks, the easy 40% is handled and they're spending attention on things only a human can judge.

### 6. Cross-review is the default

PRs are reviewed by another dev on the team. The CTO is tagged only for:

- Code in critical-path repos (current list: [auth, payments, X, Y])
- Anything you want a second senior pair of eyes on — apply the `cto-review` label
- Cases where the reviewing dev isn't comfortable signing off and escalates

This isn't a downgrade in review quality. With a small team and the CTO in meetings half the day, distributing reviews is the only way to keep them from becoming a multi-day bottleneck. The templates, DoD, and Claude pre-review exist precisely so cross-review is safe.

### 7. Review against the DoD, not vibes

As a reviewer, your job is to verify each DoD item is met—not to invent new requirements at review time. If something feels wrong but isn't in the DoD, flag it as a discussion, not a blocker. If it's something that should always be checked, propose adding it to the template.

As an author, the DoD is your contract. If you can complete every box, the work is done. No moving goalposts.

**Standard DoD** (lives on each Issue, customized as needed):

- [ ] Acceptance criteria verified
- [ ] Tests added/updated, CI green
- [ ] No new lint/type errors
- [ ] Docs updated (or n/a with reason)
- [ ] Deployed to staging
- [ ] Verified by reviewer

### 8. WIP limit: 1 in progress, 1 in review

One thing actively being coded, one thing awaiting review max.

**If you're blocked waiting on review**, in priority order:

1. Pre-flight the next ticket in your queue (highest leverage — keeps the pipeline moving)
2. Address feedback on older PRs
3. Pick something from the `good-while-waiting` label (small pre-approved chores)
4. Tooling, learning, reading unfamiliar code you'll touch next

**Don't start coding the next ticket.** It violates WIP, doubles your context-switching tax if review comes back with rework, and creates pressure to rubber-stamp reviews.

### 9. When you're stuck, surface it within 4 hours

If you're blocked—ambiguous Issue, missing access, unsure how to proceed, design question you can't resolve—comment on the Issue and tag the relevant person. Don't sit on it. Don't silently context-switch onto something else and surface it at standup three days later.

Four hours is the ceiling. Less is fine. The cost of asking early is small; the cost of grinding silently is large.

### 10. Status flows back automatically

When your PR merges and the Issue closes, a comment is automatically posted on the linked Salesforce case so support can close the loop with the customer. You don't need to update Salesforce, ping anyone, or remember to communicate.

Just make sure your PR has `Closes #123` so the linkage works. That's the only thing you have to do.

---

## Weekly friction log

Every Friday, drop one line in the `#eng-friction` Slack thread (or wherever we decide):

> "This week, the thing that wasted time / was unclear / didn't work was: ___"

Could be a template gap, a tool failure, a process step that felt useless, an Issue that should have been clearer, anything. No format. No meeting.

I read these on Friday afternoon and adjust the templates, rubrics, or this doc on Monday. This is the mechanism that keeps the system from ossifying. If you don't tell me what's broken, it stays broken.

---

## The expectations, in one place

| You are expected to | You are NOT expected to |
|---|---|
| Post a pre-flight before non-trivial work | Wait for the CTO on every review |
| Keep PRs under 1000 lines | Decompose work upfront from a vague Issue |
| Self-review before tagging a human | Update Salesforce or talk to customers |
| Address Claude's review feedback first | Take every Claude suggestion uncritically |
| Cross-review your teammates' PRs | Sign off on code outside your comfort zone — escalate it |
| Hold a WIP of 1+1 | Start the next ticket while waiting for review |
| Surface blockers within 4 hours | Grind silently |
| Drop a line in the friction log weekly | Attend a retro meeting (we don't have one) |

---

## What this is trying to achieve

- **Less context-switching.** Engineering work lives in GitHub. You don't need to open Salesforce, Monday, or Sheets to do your job.
- **Less ambiguity.** By the time you see an Issue, three layers of structuring have happened upstream. By the time you write code, your pre-flight has been thumbs-upped. By the time you ship, the DoD is met or it's not.
- **Less waiting.** Cross-review, Claude pre-review, and a 500-line PR cap together mean reviews happen in minutes, not days.
- **Less rework.** Catching ambiguity in pre-flight is roughly 100x cheaper than catching it in review, and 1000x cheaper than catching it post-merge.
- **More focused engineering time.** The whole point of the upstream system is so your time goes to actual engineering, not request-shaping.

If any of this isn't working for you, **say so.** This document is version 1.
