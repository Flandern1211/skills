# Skills

A personal collection of reusable Agent Skills. Each skill lives in `skills/<skill-name>/` and uses `SKILL.md` as its canonical entrypoint.

[中文说明](README.zh-CN.md)

## Included Skills

### submitting-pull-requests

A workflow for contributing to GitHub repositories through forks and pull requests. It covers:

- using, running, and validating a project as a real user;
- Issue selection, repository submission policy, and maintainer communication rules;
- branch setup, upstream synchronization, tests, secret scanning, and publication checks;
- recent Issue/PR format precedents and historical PR success/failure review;
- PR creation, CI, review, and post-merge cleanup;
- explicit inputs, workflow, outputs, and acceptance criteria.

Files:

- [English entrypoint](skills/submitting-pull-requests/SKILL.md)
- [中文完整版](skills/submitting-pull-requests/SKILL.zh-CN.md)

### project-governance-kit

A conversational entrypoint for Project Governance Kit (PGK). It covers:

- creating a new governed project and stopping at draft-requirement confirmation;
- inspecting an existing repository with read-only adoption checks before any writes;
- resuming an already governed project without repeated initialization or duplicate records;
- explicit trigger scenarios, inputs, workflow, outputs, acceptance criteria, and authorization gates.

Files:

- [English entrypoint](skills/project-governance-kit/SKILL.md)
- [中文完整版](skills/project-governance-kit/SKILL.zh-CN.md)
- [Codex UI metadata](skills/project-governance-kit/agents/openai.yaml)

## Repository Layout

```text
skills/
  <skill-name>/
    SKILL.md              # Canonical entrypoint (English by default)
    SKILL.<locale>.md     # Optional full localization
    agents/               # Optional UI metadata and invocation policy
    references/           # Optional on-demand documentation
    scripts/              # Optional executable helpers
    assets/               # Optional generated-output assets
```

Keep each skill self-contained. Before publishing, validate the YAML frontmatter, trigger conditions, workflow completeness, and any supporting resources.

## Languages

`SKILL.md` is the canonical file recognized by Agent Skills runtimes. This repository uses English for the canonical entrypoint and stores translated full copies as `SKILL.<locale>.md` so the runtime does not load both languages at once.

To use a localized skill, copy the relevant directory to your local skills directory and make the localization file the entrypoint. For example, in PowerShell:

```powershell
Copy-Item -Recurse .\skills\submitting-pull-requests $env:USERPROFILE\.codex\skills\submitting-pull-requests
Move-Item $env:USERPROFILE\.codex\skills\submitting-pull-requests\SKILL.md $env:USERPROFILE\.codex\skills\submitting-pull-requests\SKILL.en.md
Move-Item $env:USERPROFILE\.codex\skills\submitting-pull-requests\SKILL.zh-CN.md $env:USERPROFILE\.codex\skills\submitting-pull-requests\SKILL.md
```

On macOS or Linux, use the equivalent `cp` and `mv` commands. Do not keep two `SKILL.md` files in one directory.

## Installation

Copy the skill directory into a supported skills location:

```text
~/.codex/skills/<skill-name>/
```

Other Agent Skills-compatible runtimes may use:

```text
~/.agents/skills/<skill-name>/
```

## Contributing Skills

When adding a skill, create a dedicated directory under `skills/`, include a valid `SKILL.md`, and update the included-skills list in both README files. Never publish credentials, private configuration, session transcripts, or machine-specific files.

## License

No license has been selected yet. Add a license before redistributing or accepting external contributions.
