# Skills

A collection of reusable Agent Skills. Agents can read this repository directly; local installation is optional.

[中文说明](README.zh-CN.md)

## Usage

### Use through GitHub

Send the repository URL and your task to any Agent that can read GitHub repositories.

The Agent should:

1. inspect the repository and the descriptions in `skills/*/SKILL.md`;
2. select and fully read the matching Skill and its referenced resources;
3. follow the Skill to complete the task and state which Skill was used.

Copyable prompt:

```text
Read https://github.com/Flandern1211/skills as an Agent Skills repository.
Inspect all skills/*/SKILL.md entries, select the Skill(s) that match my task,
read the selected Skill and its referenced resources in full, then use them to
complete this task: <describe the task>
```

This works with Codex, Claude Code, Gemini CLI, GitHub Copilot, OpenCode, and other repository-capable Agents. If direct access is unavailable, let the Agent clone the repository into its workspace.

### Optional local installation

Install a Skill only when an Agent runtime requires it for automatic discovery or offline use. Follow that runtime's documentation and copy the complete Skill directory to its supported location, such as `~/.agents/skills/` or `~/.codex/skills/`.

## Available Skills

| Skill | Use | Files |
| --- | --- | --- |
| `submitting-pull-requests` | Contribute to GitHub repositories through forks and pull requests, including validation, synchronization, CI, and review. | [English](skills/submitting-pull-requests/SKILL.md) · [中文](skills/submitting-pull-requests/SKILL.zh-CN.md) |
| `project-governance-kit` | Use Project Governance Kit through conversation to create, adopt, or resume governed projects. | [English](skills/project-governance-kit/SKILL.md) · [中文](skills/project-governance-kit/SKILL.zh-CN.md) · [Optional UI metadata](skills/project-governance-kit/agents/openai.yaml) |

## Repository Structure

```text
skills/
  <skill-name>/
    SKILL.md              # Canonical entrypoint
    SKILL.<locale>.md     # Optional localization
    agents/               # Optional UI metadata
    references/           # Optional documentation
    scripts/              # Optional helpers
    assets/               # Optional assets
```

`SKILL.md` is canonical. An Agent may use a localized copy when available, but should report any conflict with the canonical entrypoint.

## Contributing

Add each Skill under `skills/<skill-name>/`, include a valid `SKILL.md`, and update both README files. Do not publish credentials, private configuration, session transcripts, or machine-specific files.

## License

This repository is licensed under the [MIT License](LICENSE).
