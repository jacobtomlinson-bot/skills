# Agent Skills

Reusable agent skills installable with the
[`skills`](https://www.npmjs.com/package/skills) CLI.

## Install

List the skills available in this repository:

```bash
npx skills add jacobtomlinson/skills --list
```

Install a specific skill:

```bash
npx skills add jacobtomlinson/skills --skill issue-writer
```

Add `--global` to make an installed skill available across projects.

## Skills

- `assemble-team` — Assemble and launch a catalog-driven software team in one
  Herdr task tab.
- `issue-writer` — Draft and revise concise, human-focused GitHub issues.
- `reconcile-board` — Reconcile Workboard cards with Herdr task tabs and GitHub
  pull requests.
- `repo-workflow` — Fork, clone, check out, or start local work on GitHub
  repositories using the preferred GitHub Flow.

Each skill lives in `skills/<skill-name>/` and contains a `SKILL.md` file with
the required `name` and `description` frontmatter.
