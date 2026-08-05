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
2. Move the card to `running`.
3. Return without waiting or polling.

Do not require every selected agent to transition to `working` before moving the card. Terminal outcomes come from the completion callback and are reconciled according to the local tools instructions.

## Safety

- Keep one project workspace and one task tab.
- Let agents manage repository isolation according to repository instructions.
- Preserve every catalog-defined approval boundary.
- Leave external review and merge authority with the human.
- If peer identity, tab membership, checkout, or gate state is ambiguous, stop before acting on the wrong target.
