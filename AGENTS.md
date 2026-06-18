# Actions

Comment-driven composite GitHub Actions that automate common PR operations. Add
them to workflows as shown in README, then trigger via PR comments.

**Language:** Nix + JS/TS

## Structure

- `command/` — Comment-driven actions (land, rebase, close, backport, run)
- `update/` — Flake input updates and publishing
- `cleanup/` — Post-merge branch cleanup
- `nix/` — Nix setup and matrix helper actions

## Comment Commands

- `.land` — Lands the current PR stack. Usage: `.land` or
  `.land | ghstack|slpr|ghpr`
- `.rebase` — Rebases the current PR on its base branch
- `.close` — Closes the current PR and optionally cleans up remote branches
- `.backport` — Backports the current PR onto a target branch. Usage:
  `.backport | <target-branch>`
- `.run` — Triggers a workflow dispatch. Usage: `.run | <workflow-name-or-path>`

## Nix Utilities

- `nix/setup` — Installs Nix and configures Cachix (optionally with QEMU)
- `nix/setup-checks-jobs` — Produces a matrix of `{ system, runner }` for checks
- `nix/setup-packages-jobs` — Produces a matrix of `{ system, runner, name }`
  for package builds

## Commit Style

- Plain-text capitalized title, no conventional-commit prefix
- Body with labels: `Design:`, `Related:`, `Closes #`
- Keep Markdown lines wrapped at 80 columns and run `nix fmt` before shipping

## Stack

- 1 commit == 1 PR via ghstack (1 commit is 1 logical atomic change)
- The commit title **is** the PR title; the commit body **is** the PR body
- Split work into stacked PRs to keep each PR small and reviewable
- To pull down an existing stack: `ghstack checkout <PR_NUMBER>`
- To update a PR: edit files, then `jj squash` (or `git commit --amend`) into
  the **target commit** of the stack — the one that PR represents; the commit
  message updates the PR title and body automatically when resubmitted
- Resubmit with `ghstack` after squashing
- `ghstack land` on the head PR to land the entire stack
- Never `gh pr merge` (creates poisoned commits)
- Never force-push ghstack branches

## Protect `main`

- Require 1 approving review
- Require linear history (no merge commits)
- Require signed commits
- Squash+rebase merge only

_Licensed under Apache-2.0. Test actions locally with `act` before submitting.
Update README when adding new actions. Always use worktrees when making
changes._
