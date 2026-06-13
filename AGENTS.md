# actions

GitHub Actions for PR management via Sapling, ghstack, and jujutsu (comment-driven).

**Language:** Nix + JS/TS

**Structure:** `{land,rebase,close,backport,update,cleanup}/` — each action; `README.md` — docs

**Commit style:** Plain-text capitalized title, no prefix. Body with labels: `Design:`, `Related:`, `Closes #`.

**Stack:** 1 commit == 1 PR via ghstack. Amend + `ghstack` to resubmit. `ghstack land` on head PR to land stack. Never `gh pr merge`. Never force-push.

**Protect `main`:** 1 review, linear history, signed commits, squash+rebase only.

*Apache-2.0. Test with `act`. Update README when adding actions*
