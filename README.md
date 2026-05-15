# .github

Cross-cutting templates, runbooks, and engineering documentation for
the `avannisllc` organization.

This repository is special: GitHub treats a repository named `.github`
at the org level as the source of organization-wide defaults. Files
placed here (Issue templates, workflow templates, the org profile,
etc.) apply across all repositories in the org without needing to be
duplicated.

If you're new to the team, start here.

## What's in here

### Engineering workflow

- **[engineering-workflow.md](./engineering-workflow.md)** — How we
  work: pre-flight, PR conventions, cross-dev review, WIP limits, the
  friction log. Read this first.
- **[workflow-reference.md](./workflow-reference.md)** — Deeper
  reference covering all three intake paths (customer, strategic,
  engineering), SLAs, scenario walkthroughs, and what lives where.
- **[ROADMAP.md](./ROADMAP.md)** — Current quarter Rocks, next quarter
  candidates, longer-horizon themes, and things we've decided not to
  do. Updated monthly.

### Procedures and conventions

- **[RUNBOOK.md](./RUNBOOK.md)** — How to do dangerous, infrequent,
  or otherwise-consequential operations. Granting Super User, running
  migrations against staging/prod, rolling back deploys, and similar.
  If you're about to do something risky, check here first.
- **[ARCHITECTURE_DECISIONS.md](./ARCHITECTURE_DECISIONS.md)** — When
  to propose an Architecture Decision before writing code, and what
  the proposal should look like. Co-authored by the team; rules apply
  to everyone.

### Issue and PR templates

- **`.github/ISSUE_TEMPLATE/`** — Templates for filing Issues. Apply
  automatically across all repos in the org. Includes:
  - `feature.md` — Feature requests and user stories
  - `bug.md` — Bug reports with reproduction steps
  - `architecture.md` — Proposals for architectural changes
- **`.github/PULL_REQUEST_TEMPLATE.md`** — Default PR template, also
  applies across all repos.

### Labels

- **[labels.yml](./labels.yml)** — Canonical label definitions for
  the org. Synced to all repos via the label sync script (see below).
  To add or change a label, edit this file and re-run the sync.
- **[scripts/sync-labels.sh](./scripts/sync-labels.sh)** — Script
  that applies `labels.yml` to every repo in the org. Run after
  editing `labels.yml`.

## How this repo is used

Most of the time, you don't open this repo directly. You experience
it indirectly:

- When you file an Issue in any org repo, the Issue templates here
  populate the form
- When you open a PR, the PR template here is the default body
- When you need labels, they exist because they were synced from
  `labels.yml` here
- When you need to remember how to do something dangerous, you come
  here to read the runbook

You open this repo directly when you want to:

- Change a template
- Add or modify a label
- Update the runbook
- Update the workflow doc or roadmap
- Add a new shared procedure

## Contributing to this repo

Changes here affect the whole org, so:

- Treat changes like real engineering work — open a PR, get review
- Especially for runbook entries, get at least one other engineer
  to read the entry before merging. The whole point of the runbook
  is that the procedure is right; a wrong runbook is worse than no
  runbook.
- For templates, test the change against a real Issue or PR in a
  test repo before merging if you can
- For labels, edit `labels.yml`, get the PR approved, then run the
  sync script to apply the changes

## Conventions

- All docs in this repo are Markdown
- Keep documents short and scannable; deep detail belongs in linked
  references, not the main doc
- Code blocks use language-tagged fences (` ```bash `, ` ```sql `, etc.)
- Filenames are UPPERCASE for canonical docs (README, RUNBOOK,
  ROADMAP) and lowercase-hyphenated for everything else
  (engineering-workflow.md, labels.yml)

## Org structure (orientation for new engineers)

Avannis runs the following primary repositories. New engineers should
know which one is which:

- **reports-monorepo** — Turborepo containing the frontend reports
  application and shared UI library (TypeScript/Turborepo)
- **reports.api** — Laravel backend for `reports.avannis.com` (PHP)
- **billing.api** — Avannis billing service: contracts, subscriptions,
  MRR ledger, invoicing (PHP)
- **jh.avannis.com** — JH-specific application (PHP)
- **awsPortal**, **portalcdk**, **samplecdk** — Infrastructure as code
  (JavaScript / Python)
- **avannis-tools**, **av-avannis-tools** — Shared internal tooling

For the full list, see the [repository overview](https://github.com/orgs/avannisllc/repositories).

## Questions

If something here is wrong, out of date, or missing, open an Issue
in this repo. The friction log
([engineering-workflow.md](./engineering-workflow.md#weekly-friction-log))
is the lightest-weight way to surface "this doc is broken" without
even filing an Issue if you don't want to.
