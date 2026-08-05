---
name: "repo-workflow"
description: "Fork, clone, check out, checkout, pull down, or start local work on GitHub repositories using the preferred GitHub Flow, remotes, paths, worktrees, and PR process."
---

# Repository workflow

Use this workflow when forking, cloning, checking out, or starting work on a GitHub repository.

## Setup

1. Resolve the upstream repository and the authenticated GitHub account with `gh auth status` and `gh api user --jq .login`.
2. Create or reuse a fork under the authenticated account. Do not create duplicate forks.
3. Clone the fork under `~/Projects/<lowercase-upstream-owner>/<repo-name>`.
   Reuse an existing checkout only after verifying that it is the intended repository and its local changes are safe to preserve.
4. Configure and verify the remotes:
   - `origin` points to the contributor fork and receives feature branches.
   - `upstream` points to the original repository and supplies the base branch and PR target.
5. Set the GitHub CLI default repository to the upstream repository with `gh repo set-default <upstream-owner>/<repo-name>`.
6. Inspect repository instructions, branch state, remotes, worktrees, and the working tree before editing. Preserve unrelated user changes.
7. If an existing fork, checkout, remote, branch, or worktree cannot be matched confidently, stop and ask rather than overwriting or repurposing it.

## Working

1. Ensure `.worktrees/` is ignored by the repository, then create or reuse a clearly matched task worktree under `.worktrees/<task-slug>` from the current upstream target branch.
2. Use a focused branch named for the issue or task inside that worktree. Keep each task's changes and checkout separate.
3. Keep commits focused, preserve required DCO/sign-off conventions, and run the repository's relevant tests, linters, and checks.
4. Push the focused branch to `origin` and open a PR from the fork branch to the upstream target branch.
5. Include the issue link, implementation summary, test evidence, and any relevant limitations in the PR.
6. Leave review and merge authority with Jacob. Do not merge the PR unless explicitly asked.

## Software-task coordination

For substantial implementation work, create a Workboard card and use the repository's existing team/Herdr workflow. Require independent tester approval before opening the PR, and reconcile the card to `review` with the PR and check evidence afterward.

## Completion checks

- The checkout path follows the standard `~/Projects/<lowercase-upstream-owner>/<repo-name>` layout.
- `origin` is the fork and `upstream` is the original repository.
- The GitHub CLI default is the upstream repository.
- Unrelated dirty work is preserved or isolated.
- The PR targets upstream and remains unmerged pending Jacob's review.
