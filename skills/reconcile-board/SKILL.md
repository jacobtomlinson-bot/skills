---
name: "reconcile-board"
description: "Reconcile Workboard cards with Herdr task tabs and GitHub pull requests."
---

# Reconcile board

Reconcile running and review Workboard cards with Herdr and GitHub. Use this skill from scheduled jobs or when asked to reconcile the board manually.

## Scope and matching

1. List every Workboard card currently in `running` or `review` across all boards.
2. For each card, identify its repository, PR, Herdr task tab, and catalog-designated completion owner conservatively from task identifiers, metadata, evidence, and live Herdr topology.
3. If an agent, task, repository, PR, reviewer authority, or feedback state cannot be matched confidently, do not guess or advance the card; record/report the discrepancy.
4. Keep reconciliation idempotent. Check existing card evidence/comments and agent state before taking action so the same review or comment is never handed off twice.

## Running cards

1. Inspect the completion owner's agent state once.
2. If it is still working, make no changes and leave it alone.
3. If it is blocked, record the blocking reason/evidence and move the card to `blocked`.
4. If it has finished or reports ready for review, verify concrete completion evidence such as the expected open PR and checks; attach useful PR/proof details and move the card to `review`, meaning ready for Jacob's human review. Do not perform a code review.

## Review cards

1. Verify the linked PR state first.
2. If the PR has been reviewed and merged and the task is clearly complete, record the merged PR URL and merge evidence, move the card to `done`, then close only the precisely matched Herdr task tab so its task agents and panes are cleaned up.
3. Never close the project workspace, an unrelated tab, or a shared/ambiguous tab. If the task-tab match is not certain, move the verified-complete card to `done` but leave Herdr untouched and record the cleanup discrepancy.
4. If the linked PR is still open, inspect submitted PR reviews and review comments for actionable, unresolved feedback authored by a repository maintainer. Treat GitHub author associations `OWNER`, `MEMBER`, or `COLLABORATOR` as maintainer evidence; if association is absent or ambiguous, verify the reviewer's repository permission and require `write`, `maintain`, or `admin`.
5. An approval with no requested work, bot output, ordinary discussion, or already-resolved feedback is not a feedback handoff.
6. When new actionable maintainer feedback exists, match the existing Herdr task tab and catalog-designated completion owner with high confidence. Prompt only that completion owner to read and address the specific maintainer feedback, coordinate the original task team and required quality gates, update the existing PR, and report through the normal completion callback. Include the PR URL and concrete review/comment identifiers or URLs.
7. Require successful prompt submission and observe the completion owner transition to `working`; only then record the feedback handoff evidence on the card and move it from `review` back to `running`.
8. If submission stalls, the owner does not transition to `working`, or the owner/tab match is uncertain, leave the card in `review` and record/report the discrepancy rather than guessing.

## Boundaries

- Do not perform code review.
- Do not prompt peers directly, restart agents, create a new team, or open a new task tab.
- Preserve approval boundaries: Jacob remains the human reviewer.
- Close only a precisely matched completed task tab; never close a workspace or ambiguous/shared tab.
