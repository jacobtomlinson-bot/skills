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

- `issue-writer` — Draft and revise concise, human-focused GitHub issues.

Each skill lives in `skills/<skill-name>/` and contains a `SKILL.md` file with
the required `name` and `description` frontmatter.
