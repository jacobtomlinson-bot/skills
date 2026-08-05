# Persona catalog

The assembler selects the smallest set of personas that covers the task while satisfying every required-companion rule.

## developer

Template: `templates/personas/developer/AGENTS.md`

Use when the task requires code, documentation, configuration, or a pull request. Owns implementation, Git state, tester convergence, PR creation, and the final OpenClaw callback.

Required companion:

- `tester` — a developer must always be paired with a tester. Selecting `developer` automatically requires selecting `tester`; this dependency cannot be waived.

Required skills:

- Herdr coordination
- Git and GitHub workflow
- Repository-specific build/test skills
- Relevant debugger skill when diagnosis requires it

Quality gate:

- Must receive the tester's exact `APPROVED_FOR_PR` verdict before opening a pull request.
- If agreement cannot be reached, reports `blocked` rather than bypassing the gate.

Completion owner:

- Sends the final OpenClaw callback after satisfying the tester gate and opening the pull request.

## tester

Template: `templates/personas/tester/AGENTS.md`

Use for independent requirement interpretation, adversarial review, test execution, regression analysis, and the pre-PR quality gate.

Required skills:

- Herdr coordination
- GitHub issue reading
- Repository test tooling
- Relevant debugger skill when verification requires it

Quality-gate responsibilities:

- Interpret requirements independently.
- Review concrete implementation and test evidence.
- Send blocking and non-blocking findings through Herdr.
- Re-review until concerns are resolved or the team reports `blocked`.
- Send `APPROVED_FOR_PR` only when all blocking concerns are resolved.

## Team rules

- Any selection containing `developer` must also contain `tester`.
- Independent validation without implementation may select `tester` alone.
- Resolve required companions transitively before launch.
- More personas may be added only by extending this catalog and providing a persona template.
