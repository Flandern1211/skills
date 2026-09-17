---
name: project-governance-kit
description: Use PGK through conversation to create new projects, adopt existing repositories, or resume governed work while preserving requirement, migration, and Git authorization gates.
---

# Project Governance Kit Agent Entry

Act as the conversational interface to Project Governance Kit. The user describes the project and
makes decisions while you run deterministic `pgk` commands and maintain governance records. User
instructions take precedence over this skill; the target repository's `AGENTS.md` governs all work
inside that repository.

This workflow requires a local `pgk` command or an accessible `project_governance` Python module.

## Trigger scenarios

Use this skill when the user wants to:

- create, start, or initialize a new project with PGK;
- let the Agent initialize PGK and lead requirement discussion;
- inspect an existing repository and adopt it into PGK safely;
- resume or continue work in a project already governed by PGK.

Do not use it merely to explain PGK, compare governance tools, read documentation, or make an
ordinary code change in a project that has not requested PGK governance.

## Inputs

Required before any write:

- the exact target project root;
- the user's intent: create a new project, adopt an existing project, or resume a governed project.

Use when already provided by the user or repository:

- project name for a new project;
- governance profile, collaboration mode, and visibility;
- an existing requirement, task, or bug identifier when resuming work.

Do not ask again for information that is already clear from the request or repository. Use Kit
defaults for optional initialization values. Ask one concise question only when the target root or
operation cannot be determined safely.

## Workflow

### Start safely

1. Resolve the exact target root. Do not initialize an ambiguous directory.
2. Inspect the directory without changing it. Check `.project-governance.toml`, `AGENTS.md`, Git
   metadata, code, and existing documents.
3. Confirm that `pgk` is available with `pgk --help`. If only repository source is available, use
   `python -m project_governance --help`. If neither works, report the installation blocker and a
   local installation command; do not claim initialization succeeded.
4. Classify the target as `governed`, `existing`, `new`, or `unclear` using the rules below.

### Classify the project

#### Governed

Treat a project as governed when it contains `.project-governance.toml` and the expected governance
entry documents. Do not run `pgk init` again. Read, in order:

1. `AGENTS.md`;
2. `docs/INDEX.md` or the configured governance index;
3. `docs/project-structure.md`;
4. `docs/STATUS.md`;
5. records linked to the current task or requested change.

Resume the active task when one exists. For new behavior, follow the repository's requirement,
design, and task chain instead of bypassing governance.

#### Existing

Treat a directory as an ungoverned existing project when it contains code, project documents, or
meaningful Git history but no PGK configuration. Run read-only discovery first:

```text
pgk adopt --root <root> --json
pgk doctor --root <root> --json
```

Summarize existing governance files, missing files, candidate mappings, sensitive findings, Git
state, and the recommended choice between supplement and migration. Stop for user confirmation. Do
not run `init --mode supplement`, `migrate approve`, or `migrate apply` merely because the user
requested an assessment or expressed future interest in PGK.

#### New

Treat an empty directory, or a directory explicitly designated for a new project without existing
business content, as new. Ask only for information that materially affects initialization: target
root, project name, and governance visibility when unclear. Use Kit defaults for profile and
collaboration mode unless the user requests otherwise.

Preview before writing:

```text
pgk init --root <root> --project-name <name> --dry-run --json
```

When the user explicitly asks to create or start the project with PGK, that request authorizes
creation of missing governance files in the named root. It does not authorize Git initialization,
commit, push, PR, merge, tag, release, deletion, or remote permission changes.

After initialization, read the generated Agent contract and governance entry documents, then run
`pgk check --root <root> --json`. Create the next available requirement record with `status=draft`,
using the user's stated project goal as its title and content. Capture goals, scope, non-scope,
assumptions, open questions, and acceptance criteria without presenting uncertainty as fact.

Stop after presenting the draft requirement and ask the user to confirm or revise it. Do not create
a design, implementation task, or business code until the requirement is accepted.

#### Unclear

If the root, project type, or write location is genuinely ambiguous, ask one short question that
resolves the ambiguity. Continue useful read-only inspection while waiting when appropriate.

## Authorization boundaries

- Existing-project supplement and migration writes require confirmation after the read-only proposal.
- Migration apply handles only explicitly approved items and follows target-repository records.
- `git init`, commit, push, PR, merge, tag, release, deletion, and remote changes retain separate
  authorization requirements. Never extend authorization for one action to another.
- Never overwrite existing project documents. Use PGK preview, conflict reporting, and proposal flows.

## Outputs

Keep the response concise and state:

- classification: `new`, `existing`, `governed`, or `unclear`;
- read-only checks and local writes performed;
- current governance stage and any blocker;
- exactly one next user decision or action.

The normal pause for a new project is requirement confirmation. The normal pause for an existing
project is adoption-plan confirmation.

## Acceptance criteria

Before reporting completion, verify all applicable conditions:

- the target is classified as `new`, `existing`, `governed`, or `unclear`;
- only actions allowed for that classification were performed;
- no existing project document was overwritten;
- no protected Git, migration, deletion, or remote action ran without matching authorization;
- a new project has governance initialized, exactly one draft requirement created, and no design,
  implementation task, or business code created before requirement acceptance;
- an existing project completed read-only adoption checks and paused for adoption-plan confirmation
  before supplement or migration writes;
- a governed project resumed recorded context without repeated initialization or duplicate records;
- the final response states classification, actions, current stage, blockers, and exactly one next
  user decision or action.

If any applicable criterion cannot be verified, report the blocker instead of claiming completion.
