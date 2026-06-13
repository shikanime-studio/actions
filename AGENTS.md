# Actions

This repository provides comment-driven agents (composite actions) that automate common PR operations. Add them to workflows as shown in README, then trigger via PR comments.

**Language:** Nix + JS/TS

## Structure

- `command/` — Comment-driven actions (land, rebase, close, backport, run)
- `update/` — Flake input updates and publishing
- `cleanup/` — Post-merge branch cleanup
- `nix/` — Nix setup and matrix helper actions

## Commit Style

- Plain-text capitalized title, no conventional-commit prefix
- Body with labels: `Design:`, `Related:`, `Closes #`
- Keep Markdown lines wrapped at 80 columns and run `nix fmt` before shipping

## Stack

- 1 commit == 1 PR via ghstack
- Amend + `ghstack` to resubmit
- `ghstack land` on head PR to land the entire stack
- Never `gh pr merge` (creates poisoned commits)
- Never force-push ghstack branches
- ghstack only works on HEAD commit chains, not detached HEADs

## Protect `main`

- Require 1 approving review
- Require linear history (no merge commits)
- Require signed commits
- Squash+rebase merge only

## Comment Commands

- `.land` — Lands the current PR stack. Usage: `.land` or `.land | ghstack|slpr|ghpr`
- `.rebase` — Rebases the current PR on its base branch
- `.close` — Closes the current PR and optionally cleans up remote branches
- `.backport` — Backports the current PR onto a target branch. Usage: `.backport | <target-branch>`
- `.run` — Triggers a workflow dispatch. Usage: `.run | <workflow-name-or-path>`

## Workflow Triggers

- Comment commands are triggered by `issue_comment` on PRs
- `workflow_dispatch` is enabled on the Commands workflow for manual testing
- Release workflow supports `workflow_dispatch` for manual releases

## Nix Utilities

- `nix/setup` — Installs Nix and configures Cachix (optionally with QEMU)
- `nix/setup-checks-jobs` — Produces a matrix of `{ system, runner }` for checks
- `nix/setup-packages-jobs` — Produces a matrix of `{ system, runner, name }` for package builds

## Backport Strategy

- `ghstack` method: unlinks the stack, rebases onto target, resubmits
- `sapling` method: uses sapling pr unlink/rebase/submit
- `github-pull-request` method: uses plain git cherry-pick (no jj dependency). Creates a `backport/<target>/<head>` branch and opens a PR

## Update Strategy

- `ghstack`/`sapling` methods: use respective tools for submission
- `github-pull-request` method: uses plain git (no jj dependency). Creates a timestamped `update/flake-<datetime>` branch and opens a PR. Skips if no changes.

*Licensed under Apache-2.0. Test actions locally with `act` before submitting. Update README when adding new actions*
