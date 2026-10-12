---
name: "assemble-team"
description: "Assemble and launch a catalog-driven software team in one Herdr task tab."
---

# Assemble team

Use this skill when delegating software work through Herdr.

## Inputs

Resolve before launch:

- Workboard card ID and current status
- Repository root
- Source issue or complete task brief
- Existing project Herdr workspace
- Short task slug
- Required deliverable

Read `references/persona-catalog.md` completely. Treat it as the sole authority for available personas, selection rules, required companions, responsibilities, skills, quality gates, and completion ownership. Resolve persona template paths relative to this skill and pass each selected template's absolute path in that agent's initial prompt.

## Herdr access context

This skill may coordinate Herdr from either inside or outside a Herdr-managed pane. Its bounded remote-coordination branch is an explicit exception to the generic `herdr` skill's `HERDR_ENV=1` guard.

When `HERDR_ENV=1`, follow the `herdr` skill normally. When it is absent:

1. Verify that the Herdr CLI can list workspaces, then resolve the exact project workspace by repository root. CLI connectivity plus an unambiguous repository match authorizes only the bounded operations below.
2. Use explicit workspace, tab, pane, and agent IDs for every command. Never use focused-session defaults, `--current`, or inherited `HERDR_*` identifiers.
3. Limit remote coordination to:
   - read-only preflight and status checks for the matched project workspace;
   - one-off creation of one task tab and its team panes, agent launches, initial briefs, operator screensaver, and completion-owner handoff verification;
   - a human-requested follow-up to an unambiguously matched idle team, including its Workboard transition, prompt submission to the completion owner, and transition-to-`working` verification.
4. Preserve UI focus with `--no-focus`. Leave unrelated workspaces, tabs, panes, and agents untouched, and leave cleanup to an inside-Herdr operator unless the human explicitly requests exact-target cleanup.
5. If the Herdr CLI is unavailable, the repository-to-workspace match is ambiguous, or an exact target cannot be resolved, stop before mutation and report the blocker.

Remote access is ready when the exact project workspace is resolved and every planned command has an explicit target.

## Existing-team preflight

Before selecting personas, inspect the repository's existing Herdr workspace, task tabs, and live agents for a team already covering this task. Match conservatively using the Workboard card ID, issue or PR URL/number, task slug, repository, and task brief.

1. If no credible match exists, record that the preflight found no existing team and continue to team selection.
2. If a matching or materially overlapping team exists, report its task tab, agents and states, linked PR or issue, and the evidence used to match it.
3. Stop before creating panes, starting agents, sending work prompts, or cleaning up the candidate team. Ask the human to choose one of: reuse/nudge the existing team; create a deliberately parallel replacement team; or clean up the old team and start fresh.
4. Resume only after the human makes that choice. Preserve the chosen team's existing checkout, tab, and agent state unless the human explicitly selected cleanup.
5. If identity, task overlap, or tab ownership is ambiguous, treat it as an existing-team match and ask for direction rather than risking duplicate work.

## Workboard transitions

Every Herdr work interval must have a matching Workboard lifecycle interval. When a card must move to `running`:

1. Verify the exact card ID and its current status first.
2. Use a supported Workboard command or API when available.
3. If no transition command is exposed, manually update the exact card in the Workboard SQLite database in one transaction: update the card status and timestamps, and insert a `moved` audit event with the previous and new status. Never leave a directly launched Herdr team represented as `todo` merely because the CLI lacks an update command.
4. Verify the result with `openclaw workboard show <card-id>` before proceeding.

## Existing-team reactivation

When the human chooses reuse/nudge or explicitly asks an idle team to perform follow-up engineering work, treat it as a new work interval, not as a comment on a completed team. This branch may use the bounded remote-coordination exception above.

1. Verify the matched completion owner is `idle` or `done`. If it is already `working` or `blocked`, report that state instead of stacking a new prompt.
2. Inspect the Workboard card and select the lifecycle path before submitting a Herdr prompt: `review` → `running` with reactivation evidence; `todo` → `running` with the existing-team match; or the defined blocked path for a blocked card.
3. Apply the Workboard transition procedure before prompting the team; record the existing-team match and the human's follow-up request as handoff evidence.
4. Submit the follow-up through the existing team’s designated completion owner, then verify that owner transitions to `working`. If submission fails or the owner does not transition, restore or report the card’s accurate state rather than leaving it falsely `running`.
5. On the owner’s completion callback, reconcile the new evidence normally: `running` → `review` for an open deliverable ready for Jacob’s review, or apply the blocked/failed path.

Reactivation is complete when the completion owner is working on the submitted follow-up and the Workboard card records the matching active interval.

## Team selection

1. Match task capabilities to catalog entries.
2. Select the smallest team satisfying every applicable catalog rule and dependency.
3. Close required-companion dependencies transitively until the team is complete.
4. Do not invent unavailable personas. Surface uncovered capabilities before launch.
5. Record the selected personas and rationale on the Workboard card or in the task handoff.

Selection is complete when every task capability and every catalog dependency is covered.

## Herdr task launch

Use the `herdr` skill's task-scoped tab topology to create the requested task tab and agent panes. Do not duplicate its workspace, tab, CWD, pane, focus, worktree, ID-handling, or CLI rules here.

Team-specific constraints:

- Launch every selected persona as a separately named Codex agent.
- Name each agent `<task-slug>-<catalog-role-slug>`.
- Keep the entire selected team in the one task-scoped tab created through the `herdr` skill.
- Reserve the tab's initial root pane as an agent-free operator pane.
- After launching every agent pane, use the `herdr` skill's operator-pane procedure to run `screensaver` in the root pane without changing focus.

## Initial briefs

Every agent brief must include:

- Absolute persona `AGENTS.md` path and an instruction to read it completely before acting
- Same issue URL or complete task statement
- Repository root
- Workboard card ID
- Task tab ID
- Own Herdr agent name and peer agent names
- Expected deliverable
- Catalog-defined responsibilities, dependencies, communication, and quality gates
- Herdr submission rule: use `herdr pane run <pane-id> "<message>"` for atomic text-plus-Enter submission; `herdr agent send` only writes literal text and must not be used to submit prompts or peer messages
- OpenClaw completion callback contract when assigned by the catalog

Briefing is complete when every selected agent has the same task context and can identify its catalog-defined obligations and peers.

## Coordination

Use Herdr for event-driven peer messages.

- Resolve the peer's pane ID and submit each message with `herdr pane run <pane-id> "<message>"` so text and Enter are sent atomically.
- Target peers by unique agent name when inspecting state or resolving their pane.
- Include enough context and evidence for the recipient to act.
- Follow the catalog and persona templates for message order, review rounds, verdicts, and escalation.
- Let peers respond through Herdr rather than continuously polling them.

`herdr agent send` writes literal text without Enter. Do not use it to submit prompts or peer messages.

Do not require every peer to be observed in the `working` state. Initial handoff verification is limited to the catalog-designated completion owner.

## Quality gates and completion

Apply every gate and convergence condition defined by the catalog and selected persona templates. A deliverable may proceed only after all required verdicts exist. If the team cannot satisfy a gate or resolve a material disagreement, use the catalog-defined blocked path.

Only the catalog-designated completion owner sends the final OpenClaw callback. The callback must include the Workboard card ID, outcome, deliverable URL when present, summary, checks, and evidence for every required verdict. Follow the callback reconciliation policy in the local tools instructions.

## Workboard lifecycle

After all briefs are submitted:

1. Verify the catalog-designated completion owner transitioned to `working`; correct a missed submission if needed.
2. Apply the Workboard transition procedure to move the card to `running`.
3. Return without waiting or polling.

Do not require every selected agent to transition to `working` before moving the card. Terminal outcomes come from the completion callback and are reconciled according to the local tools instructions.

## Safety

- Keep one project workspace and one task tab.
- Let agents manage repository isolation according to repository instructions.
- Preserve every catalog-defined approval boundary.
- Leave external review and merge authority with the human.
- If peer identity, tab membership, checkout, or gate state is ambiguous, stop before acting on the wrong target.
