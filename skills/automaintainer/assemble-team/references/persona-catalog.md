# Persona catalog

The assembler selects the smallest set of personas that covers the task while satisfying every required-companion rule.

## developer

Template: `templates/personas/developer/AGENTS.md`

Use when the task requires code, documentation, configuration, or a pull request. Owns implementation, Git state, quality-gate convergence, PR creation, and the final OpenClaw callback.

Required companion:

- `tester` — a developer must always be paired with a tester. Selecting `developer` automatically requires selecting `tester`; this dependency cannot be waived.

Required skills:

- Herdr coordination
- Git and GitHub workflow
- Repository-specific build/test skills
- Relevant debugger skill when diagnosis requires it

Quality gate:

- Must receive every assigned quality-gate verdict before opening a pull request: `APPROVED_FOR_PR` from the tester and, when assigned, `SECURITY_APPROVED_FOR_PR` from the security tester.
- If agreement cannot be reached, reports `blocked` rather than bypassing the gate.

Completion owner:

- Sends the final OpenClaw callback after satisfying all assigned gates and opening the pull request.

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

## security-tester

Template: `templates/personas/security-tester/AGENTS.md`

Use when a task changes CI/CD, release, deployment, authentication, secrets, permissions, network exposure, or other security-sensitive configuration.

Required skills:

- Herdr coordination
- GitHub Actions and repository configuration review
- Dependency action pinning and least-privilege review

Quality-gate responsibilities:

- Inspect workflow permissions, token/secrets usage, third-party actions, event triggers, and release publication paths.
- Send blocking and non-blocking findings through Herdr.
- Re-review until blocking concerns are resolved.
- Send `SECURITY_APPROVED_FOR_PR` only when configuration is least-privilege and no material vulnerability or unsafe misconfiguration remains.

## Team rules

- Any selection containing `developer` must also contain `tester`.
- Independent validation without implementation may select `tester` alone.
- When `security-tester` is selected, the developer must receive both tester and security-tester approval verdicts before creating a pull request.
- Resolve required companions transitively before launch.
- More personas may be added only by extending this catalog and providing a persona template.
