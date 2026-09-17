---
name: submitting-pull-requests
description: Use when contributing to a GitHub repository through a fork and pull request, including fork/clone setup, branch creation, upstream synchronization, feature-branch publication, opening or updating a PR, resolving review or CI failures, and deciding whether a change is ready to merge. Also trigger for requests such as “提 PR”, “开合并请求”, “提交贡献”, or “同步 fork”.
---

# Submitting Pull Requests

## Overview

Use this workflow to turn a local change into a reviewable, reproducible GitHub Pull Request (PR). The basic path is **Fork → Clone → branch → synchronize upstream → push → PR**. The safeguards below cover real-user validation, repository conventions, issue selection, scope, tests, secret scanning, CI, review, and cleanup.

## Operating Rules

- Treat the upstream repository as the source of truth for the PR base. `origin` normally means the contributor's fork; `upstream` means the original repository.
- Never make a contributor change directly on `main`/`master` or push a feature commit to the fork's default branch. Preserve existing work before moving it.
- Do not publish a branch, open a PR, force-push, merge, delete a branch, or change repository settings unless the user explicitly requested that action or confirms it when reached. Preparation may proceed without publication.
- Never bypass required CI, branch protection, review, hooks, or security checks to satisfy a deadline.
- If credentials appear in a worktree, diff, PR, commit, CI log, or artifact, stop normal PR work. Revoke or rotate them first, then clean the worktree and any affected history. Removing only the current line does not undo a leak.
- Prefer reversible operations. For a rewritten personal PR branch, use `--force-with-lease`, never unguarded `--force`.
- Whether maintainer approval is required before development or PR submission is determined by the repository's official contribution guide, Issue template, labels, and maintainer-published rules. If they require claiming, assignment, or prior approval, comply. If they do not, a suitable Issue may be implemented and linked directly from a PR.
- Treat real project use as the primary evidence source. Do not hunt for code-only issues without demonstrated impact on runtime behavior, users, tests, builds, security, or maintainability.

## Inputs

Before using this skill, obtain or determine the following from the repository. Mark unavailable items as unknown or blocked in the final output; never invent them:

- upstream repository, target base branch, GitHub account, and fork;
- target users and the user role being simulated, such as end user, developer user, maintainer, or contributor;
- target Issue, target module, or an evidence-backed problem statement;
- README, contribution guide, Issue/PR templates, commit and sign-off rules, CODEOWNERS, and test instructions;
- official rules for Issue claiming, assignment, maintainer approval, and PR timing;
- local runtime, required configuration, dependencies, startup procedure, logs, and reproduction evidence;
- current branch, worktree status, remotes, and target PR base;
- related accepted Issues/PRs from the last 15 days, plus historical successful and unsuccessful PR samples;
- explicit user authorization for pushing branches, creating/updating PRs, or other public actions.

## Workflow

### 1. Establish the repository and contribution target

Ask for or determine:

- upstream repository and target base branch, usually `OWNER/REPO:main`;
- the user's GitHub account and fork;
- the project's main user groups and the real user role to simulate;
- intended change and linked Issue, if any;
- project-specific contribution, test, sign-off, and PR-template rules.

If the user has not forked the repository, create the fork on GitHub and verify that it belongs to the intended account. Do not guess the repository URL or default branch.

For an existing checkout, run:

```bash
git rev-parse --show-toplevel
git remote -v
git branch --show-current
git status --short --branch
```

For a new checkout, clone the fork and enter it:

```bash
git clone https://github.com/<USER>/<REPO>.git
cd <REPO>
```

### 2. Synchronize, install, run, and use the project as a real user

Before submitting an Issue or PR, confirm that the local checkout reflects the latest target branch, then follow the project's own documentation to understand and exercise it:

```bash
git fetch upstream --prune
git fetch origin --prune
git switch <default-or-target-branch>
git pull --ff-only upstream <default-or-target-branch>
git status --short --branch
```

Identify the user represented by the investigation: end user, developer user, maintainer, or contributor. Read the current README, user documentation, contribution guide, Issue template, PR template, commit/sign-off rules, and test instructions. Follow the official recommended path from a first-time user's perspective to install dependencies, configure the environment, start the project, and use its core functions. Exercise the complete user journey related to the target capability, module, or suspected problem instead of skipping setup and calling internal code directly.

Do not assume the code is defective and do not treat source files as an issue checklist. Observe and record problems a real user might encounter:

- installation, configuration, or startup steps that fail or are difficult to understand;
- errors, crashes, anomalous logs, weak error messages, or inconsistent behavior;
- discrepancies between documentation and behavior, missing critical steps, or ambiguous terminology;
- confusion, repeated effort, undiscoverable functionality, or friction in a core flow;
- test, build, collaboration, upgrade, or troubleshooting costs imposed on developers, maintainers, or contributors.

Record the user role, exact commands, environment and configuration, versions, action path, reproduction steps, actual result, expected result, logs, test results, and verified scope. A code location may support the evidence but cannot be the sole reason for a contribution.

Only after real use, runtime execution, tests, logs, documentation discrepancies, or maintenance workflows reveal a reproducible, explainable, verifiable problem may you inspect the relevant code and propose a fix. Do not change code merely because it looks untidy or theoretically improvable when there is no demonstrated impact on project execution, user experience, tests, builds, security, or future maintenance. Code-quality, missing-test, build, documentation, and maintainability findings are valid only when their concrete effect on development, testing, builds, collaboration, upgrades, or troubleshooting is explained.

If the real user journey, relevant tests, and maintenance checks reveal no worthwhile problem with actual impact, record **no suitable issue found**, list the user role, flows, and verified scope, and stop. Do not keep scanning unrelated code, inflate minor details, or force an Issue or PR for the sake of contributing.

If the project cannot run, record the blocking cause, missing configuration or dependency, exact error output, attempted commands, and actual verified scope. Do not claim real-use validation or imply successful testing.

All Issue, commit, and PR titles and bodies must follow project conventions, including required templates, linking syntax, test sections, and commit format. Official requirements take precedence over historical examples; re-check them before publication.

### 3. Establish an Issue-first work item and apply repository submission rules

Before creating a branch or writing implementation code, inspect open Issues and choose an unassigned Issue aligned with the target capability, module, or project direction. Check assignees, discussion, and linked work; an empty assignee field alone does not prove the Issue is unclaimed:

```bash
gh issue list --repo <OWNER>/<REPO> --state open --limit 100 --json number,title,body,labels,assignees,url
gh issue view <ISSUE_NUMBER> --repo <OWNER>/<REPO> --comments
```

Then determine whether official repository rules require advance claiming, assignment, maintainer approval, or design approval. Only when the rules require it should this become a gate before development or PR submission. In that case, comment on the Issue first:

```text
My understanding of the problem: <specific problem and impact>
Planned approach: <implementation direction and scope>
May I take responsibility for implementing this: <please confirm or suggest changes>
Existing design or implementation constraints: <known constraints; please confirm or add>
```

Record the Issue URL/number, assignee or claim evidence, applicable official rule path or link, comment URL, and any reply or assignment required by the rule. If maintainer approval is required, verify the responder using repository permissions, CODEOWNERS, or the documented maintainer list. A contributor, bot, or unauthenticated reaction does not satisfy such a requirement.

Use these submission-readiness states:

- `ready`: required claiming, assignment, or approval is satisfied, or the repository has no prior-approval requirement. Formal development and an associated PR may proceed.
- `waiting`: official rules require claiming, assignment, or approval and it is not yet satisfied. Stop formal development, commits, pushes, and PR creation/update.
- `needs clarification`: official rules conflict or do not make the approval requirement clear. Ask maintainers; do not choose the loosest interpretation.

If the README, contribution guide, Issue template, labels, and maintainer-published rules do not require prior approval, do not wait indefinitely for a maintainer reply. Briefly state the intended work and approach on the Issue, then develop and open an associated PR according to project rules. User authorization for the public action is still required separately.

If no suitable unassigned open Issue exists, do not take an assigned Issue without repository policy or maintainer direction, and do not invent a PR target. Use the project as described above. Inspect related code, tests, documentation, and history only when real use, tests, logs, documentation discrepancies, or maintenance workflows have produced a clear problem signal. Without such a signal, finish with **no suitable issue found** and do not scan code merely to create a PR.

For a newly discovered problem, create an Issue that includes the target user or maintainer role, evidence source, problem statement, impact and scope, reproduction steps, actual and expected results, logs/test results/code locations, proposed solution, validation performed, and confirmation of Issue-template compliance. Center the narrative on a real user problem or concrete maintenance cost, not merely code that looks suboptimal. Then apply official rules: wait if they require approval or assignment; otherwise development and an associated PR may proceed directly.

For work already tied to an Issue, announce the start as required by project rules, post material progress updates, and preserve comment links. If scope or direction changes materially, re-check the official rules and readiness state.

### 4. Reference recent accepted Issue and PR formats without overriding official conventions

Before drafting or updating an Issue or PR, read the repository's own rules first: `.github/ISSUE_TEMPLATE/`, `PULL_REQUEST_TEMPLATE.md` or `.github/pull_request_template.md`, `CONTRIBUTING.md`, contribution documentation, `CODEOWNERS`, commit/sign-off guidance, and CI checks that validate titles, bodies, or commits. Official instructions are the mandatory baseline and always override examples.

Then inspect accepted precedents so wording and evidence granularity match current practice:

```bash
gh pr list --repo <OWNER>/<REPO> --state merged --search "<module or change keywords> merged:>=<YYYY-MM-DD>" --limit 20
gh issue list --repo <OWNER>/<REPO> --state closed --search "<module or issue keywords> closed:>=<YYYY-MM-DD>" --limit 20
```

Prefer examples merged or confirmed within the last 15 days and related to the same change type, module, scope, or risk. Read titles, descriptions, templates, test/verification sections, linked Issues, and maintainer comments. Distinguish a maintainer-accepted Issue from one closed as stale, duplicate, rejected, or unexplained; only accepted examples establish a format precedent.

Apply this precedence order:

1. Repository-official templates, contribution rules, and automated format checks — mandatory.
2. Maintainer decisions in recent accepted Issues/PRs — authoritative guidance for gaps.
3. Merged or confirmed examples from the last 15 days — preferred practical reference for wording, section order, and evidence depth.
4. Older examples or generic GitHub conventions — use only when no recent accepted precedent exists, and report the limitation.

Choose examples closest in change type, module, scope, and risk rather than simply the newest. Never copy a recent example over an official requirement, treat an unmerged or merely closed item as formatting authority, or invent conventions from a template gap. If official rules are ambiguous or recent accepted samples are absent, report the missing evidence instead of guessing. In the final report, separate mandatory rules consulted from current-practice examples borrowed.

### 5. Configure and verify remotes

Ensure the fork is the push target and the original repository is available as `upstream`:

```bash
git remote get-url origin
git remote get-url --push origin
git remote -v
```

If `git remote -v` does not show `upstream`, add it:

```bash
git remote add upstream https://github.com/<OWNER>/<REPO>.git
git remote -v
```

If an existing `upstream` URL is wrong, show the mismatch and ask before changing it. Do not copy shell-specific null redirection into a different shell.

### 6. Create a focused working branch

Use a short branch name such as `feat/<topic>`, `fix/<topic>`, `docs/<topic>`, or the project's required convention:

```bash
git switch -c <type>/<short-topic>
```

If the current branch is the default branch and has **uncommitted** changes, creating the new branch directly preserves them. If changes are already committed on local `main`, create a branch at that commit before considering a reset. Never discard work merely to make the branch clean.

Keep one coherent change per branch. Check status after branch creation.

### 7. Develop and validate locally

Read the contribution guide, test configuration, CODEOWNERS, PR template, and CI workflow. Implement only the confirmed scope. Before staging:

```bash
git status --short
git diff --stat
git diff
git ls-files --others --exclude-standard
```

Check for secrets, private data, generated files, large binaries, debug output, and unrelated edits. Do not print a suspected secret into chat or a public PR. Run the repository's required formatter, linter, type check, build, unit tests, integration tests, and security scan as applicable. Record exact commands and results.

Stage selectively and inspect the staged diff:

```bash
git add <files>
git diff --cached --stat
git diff --cached
```

Use a concise imperative commit message that matches project conventions. Do not use `--no-verify` unless a maintainer explicitly requires it.

### 8. Synchronize upstream before publication

Do this before the first push and again immediately before opening or updating a PR:

```bash
git fetch upstream --prune
git fetch origin --prune
git switch <feature-branch>
git log --oneline --decorate -5 upstream/<base-branch>
```

If the branch has uncommitted changes, commit them or stash them together with untracked files before synchronizing. Choose rebase or merge according to project policy:

```bash
# Linear history; only when the branch is fully yours to rewrite:
git rebase upstream/<base-branch>

# Preserve history or use when the branch is shared:
git merge --no-edit upstream/<base-branch>
```

For conflicts, inspect `git status` and `git diff --name-only --diff-filter=U`, resolve deliberately, remove all conflict markers, stage each resolved file, then continue with `git rebase --continue` or `git commit`. If the operation is wrong, abort with `git rebase --abort` or `git merge --abort`. After the final synchronization, whether or not conflicts occurred, confirm that the branch is not behind `upstream/<base-branch>` and rerun all relevant tests. Record the updated base commit and fresh test results.

### 9. Commit and publish the feature branch

Before committing or pushing, confirm that the submission-readiness state remains `ready`, repository policy has not introduced a new claim or approval requirement, and required Issue progress updates are recorded. If the state is `waiting` or `needs clarification`, stop without committing or pushing. Then verify the branch, staged scope, tests, and target remote again:

```bash
git status --short --branch
git diff --cached
git commit -m "<imperative summary>"
git show --stat --oneline HEAD
git push -u origin <feature-branch>
```

If a rebase changed an already-published personal PR branch, compare local and remote history first. Only with user authorization and no other contributor's work use:

```bash
git push --force-with-lease origin <feature-branch>
```

If publication was not explicitly requested, stop after preparing the commands and ask for confirmation.

### 10. Review relevant historical PRs before creating or updating a PR

Run this step immediately before creating a new PR or updating an existing one. Use GitHub search, the repository PR list, and authenticated `gh` to find examples related to the current change type, target module, or problem scenario:

```bash
gh pr list --repo <OWNER>/<REPO> --state merged --search "<module or issue keywords>" --limit 20
gh pr list --repo <OWNER>/<REPO> --state closed --search "<module or issue keywords>" --limit 20
```

Build two explicitly labeled samples:

- **Successful sample:** at least 3 PRs with state `MERGED`. Verify merge date and merge commit; a merely closed PR is not successful.
- **Unsuccessful sample:** at least 3 related PRs closed without merge, rejected, declined, or otherwise not accepted. Record the exact state, such as `CLOSED`, `DECLINED`, `REJECTED`, or the platform equivalent, and never describe them as merged.

For every sampled PR, inspect changed-file scope, implementation approach, tests and check results, PR description, commit messages, review comments and decisions, and stated or evidenced failure reasons. Produce a compact comparison covering:

1. recurring scope boundaries and implementation patterns;
2. test depth, missing coverage, and CI/review outcomes;
3. description and commit-message traits that helped or hurt review;
4. common review objections, rework, and failure causes.

Convert the comparison into a **pre-publication checklist for the current PR**, covering scope, implementation, tests/CI, description, commits, and review-risk items. Execute each item before proceeding. Preserve links or IDs for all references and report the evidence in the final handoff.

If fewer than 3 relevant merged PRs or fewer than 3 relevant unsuccessful PRs can be found, explicitly report **sample insufficient**, including search scope, counts, and missing categories. Do not invent patterns or imply more confidence than the evidence supports. Turn only directly supported observations into checklist items.

Record both practices worth reusing from successful PRs and pitfalls to avoid from unsuccessful PRs. Repeat this review before updating an existing PR when its scope, implementation, tests, or review context changes materially.

### 11. Verify the exact PR diff and create or update the PR

Compare against the upstream base, not the fork's possibly stale default branch:

```bash
git status --short --branch
git log --oneline upstream/<base-branch>..HEAD
git diff --stat upstream/<base-branch>...HEAD
git diff upstream/<base-branch>...HEAD
```

Create or update the PR only after confirming:

- official Issue claiming, assignment, or approval requirements are satisfied; when none exist, `ready` and the supporting policy check are recorded;
- the project was synchronized to the latest target branch before investigation and again immediately before publication;
- real-use, runtime, test, documentation, or maintenance evidence is recorded, or the runtime blocker and unverified scope are explicitly reported;
- focused code self-review and any project-required peer review are complete;
- relevant tests and checks passed with exact evidence recorded;
- user-facing and developer documentation is synchronized, or a documented reason explains why no documentation change is needed;
- the **base repository** is the original upstream repository;
- the **base branch** is the intended target branch;
- the **head repository** is the user's fork;
- the **head branch** is the feature branch just pushed;
- commits and files contain only the intended change;
- tests and local checks are recorded.

Use GitHub's compare page or `gh` when available:

```bash
gh auth status
gh pr create --repo <OWNER>/<REPO> --head <USER>:<feature-branch> --base <base-branch> --title "<title>" --body-file <pr-body.md>
```

The PR body must follow the repository template exactly and explain the change from the perspective of the affected user or concrete maintenance cost. Include target user or maintainer role, evidence source, reproduction steps, actual and expected results, logs/test results, impact, implementation scope, validation commands and results, `Fixes #<Issue-number>` or the required association syntax, compatibility or migration notes, known limitations, and confirmation of Issue/PR/commit convention compliance. Do not merely say that code looks suboptimal without explaining real impact. Re-check the official template and the accepted precedents from the last 15 days found in step 4. After creating a PR for an Issue, comment on the Issue with the PR URL, change summary, and test results.

### 12. Handle CI and review

After opening or updating the PR, inspect required checks, reviewers, labels, mergeability, and changed files. Address review feedback with focused commits on the same branch and push; the PR updates automatically. Explain what changed and which checks were rerun.

If CI fails, identify the first actionable failure and reproduce it locally where possible. Fix the cause, rerun required checks, and update the PR. Never hide a failure by skipping a job, weakening a test, disabling branch protection, or merging manually.

If a secret was exposed, keep the PR blocked until the credential is revoked or rotated, exposure is assessed, affected worktree/history/artifacts are cleaned, security owners are notified, and required CI and review pass. Put incident details in a restricted security channel, not the public PR.

### 13. Close out after merge or rejection

Only after the user or maintainer confirms that the PR is merged or closed:

```bash
git switch <default-branch>
git fetch upstream --prune
git pull --ff-only upstream <default-branch>
git branch -d <feature-branch>
```

Delete the remote feature branch only with explicit authorization and only when it is no longer needed. Keep a short record of the PR URL, final commit, checks, review result, and follow-up Issues.

## Acceptance Criteria

Before creating or updating a PR, evaluate every criterion below. If an item does not apply, state why instead of silently omitting it:

- required input is available, or unavailable information and its impact are explicitly marked;
- the project was synchronized and used in a realistic environment, or runtime blockers, errors, and unverified scope are accurately recorded;
- the target user or maintainer role is identified, and the official installation, configuration, startup, and relevant core flow were exercised from a new-user perspective;
- the Issue or problem has reproducible, explainable, verifiable evidence and affects runtime, user experience, tests, builds, security, or future maintenance;
- official submission rules were checked and readiness is `ready`: required claiming, assignment, or approval is satisfied, or the repository was confirmed to have no prior-approval requirement;
- Issue, commit, and PR content follows official templates and submission rules and references applicable recent accepted precedents;
- scope is focused, code self-review, secret scanning, relevant tests, and documentation synchronization are complete;
- the latest target branch was synchronized again before publication, the branch is not behind, and relevant tests were rerun;
- historical PR review is complete; insufficient samples are reported with counts, scope, and conclusion limits;
- all applicable Required Output fields contain evidence, with unknown, waiting, and risk items explicit;
- pushing or creating/updating a public PR has explicit user authorization.

If realistic use and related maintenance checks reveal no worthwhile problem, the correct outcome is **no suitable issue found**. Report the user role, exercised flows, and verified scope, then stop. The absence of an Issue or PR is not a failure and must not trigger forced issue hunting.

Acceptance results:

- `PASS`: all applicable mandatory criteria are satisfied; authorized commit or PR actions may proceed.
- `NO_ISSUE`: realistic use and maintenance checks found no worthwhile problem; report scope and finish without an Issue or PR.
- `WAIT`: repository policy requirements or user authorization are pending, or rules require clarification; report the wait and do not perform the corresponding public action.
- `FAIL`: critical evidence is missing, official rules/tests/synchronization were skipped, secrets were exposed, or another material problem exists; fix it and re-evaluate.

## Required Output

Report progress as a checklist, explicitly marking unknown or blocked items:

```text
- [x] Repository/remotes: origin=<fork>, upstream=<source>, base=<branch>
- [x] Runtime-first validation: sync=<command/result>; docs/config/dependencies=<read/configured>; real-use flow=<scope/result> (or blocker=<exact error and unverified scope>)
- [x] User perspective: role=<end user/developer/maintainer/contributor>; new-user journey=<install, configure, start, core flow>; observations=<friction, errors, confusion, inconsistencies, documentation gaps, or no suitable issue>
- [x] Format authority: official templates/contribution rules=<paths or `none found`>; recent accepted precedents=<Issue/PR links dated ≤15 days, or `no recent accepted precedent`>; hierarchy=<official then recent examples>
- [x] Issue and submission policy: <Issue URL/number and unassigned evidence, or new Issue URL>; official claim/approval rule=<path/link or `none`>; readiness=<ready/waiting/needs clarification>; evidence=<comment/assignment/approval link or no-wait basis>
- [x] Issue communication: <pre-start comment, progress updates, and outstanding questions>
- [x] Working branch: <branch>
- [x] Scope/diff reviewed; secret check: <result>
- [x] Tests/checks: <commands> — <results>
- [x] Synchronization: <rebase/merge/none>, conflicts: <result>
- [x] Commit/push: <commit>, <remote branch> (or awaiting authorization)
- [x] Historical PR review: merged=<3+ links/IDs>; unsuccessful=<3+ links/IDs with exact states> (or `sample insufficient` with counts and search scope)
- [x] Reusable practices: <evidence-backed summary>
- [x] Pitfalls to avoid: <evidence-backed summary>
- [x] Pre-publication checklist: <Issue/policy, code review, tests, docs, history review, scope, description, commits — executed; blockers listed>
- [x] Issue/PR evidence: role=<target user/maintainer>; source=<real use/test/log/docs discrepancy/Issue/maintenance risk>; reproduction=<steps>; actual=<result>; expected=<result>; impact=<user friction or maintenance cost>; logs/tests/code location=<evidence>; scope=<files/components>; validation=<commands/results>; conventions=<compliant or exception>
- [x] PR: <URL>, base/head verified; Issue association=<`Fixes #...` or project syntax>; Issue follow-up comment=<URL>
- [ ] CI/review/merge: <current status and next action>
- [x] Acceptance: <PASS/NO_ISSUE/WAIT/FAIL>; basis=<satisfied, no issue, waiting, or failing items>
```

When a PR exists, end with its URL, exact test evidence, current CI/review state, remaining risks, and next action. Never claim a PR is ready or merged without evidence.

## Common Mistakes

| Mistake | Correction |
|---|---|
| Pushes to `main` or opens a PR from the wrong repository | Create a feature branch and explicitly verify base and head |
| Treats `origin` as upstream | Keep `origin` as the fork push target; fetch the original through `upstream` |
| Synchronizes against stale `origin/main` | Fetch and compare against `upstream/<base-branch>` |
| Uses unguarded `--force` after rebase | Confirm branch ownership and use `--force-with-lease` |
| Treats a closed PR as successful or samples only convenient examples | Verify `MERGED` and the merge commit; label unsuccessful states separately |
| Skips historical review because search is inconvenient or time is short | Report `sample insufficient` with counts and scope; never invent conclusions |
| Repository rules require claiming or approval, but development or PR submission begins anyway | Satisfy required claiming, assignment, or approval first; proceed directly only when rules allow it |
| Repository rules do not require approval, but work waits indefinitely for a maintainer reply | Record the policy check and `ready` basis, state the direction on the Issue, then submit the associated PR according to project rules |
| Creates a speculative PR when no Issue exists | Create an evidence-backed Issue first and follow official submission rules; do not invent a problem |
| Mentions an Issue only in the PR title or forgets the follow-up | Use the required association syntax and reply on the Issue with PR URL, summary, and tests |
| Treats tests, user permission, a deadline, or an emoji as an approval required by repository policy | Wait for maintainer approval only when official rules require it; otherwise record the no-wait basis |
| Assumes passing tests imply documentation and self-review are complete | Make code review and documentation synchronization explicit checklist items |
| Starts by scanning code for theoretical defects | Synchronize, read instructions, run the project, and exercise the relevant user journey before targeted code review |
| Continues searching code merely to produce a contribution after realistic use found no problem | Report **no suitable issue found**, the user role, and verified scope, then stop |
| Describes only why code looks suboptimal | Frame the Issue/PR around real user friction or maintenance cost and attach reproduction, actual/expected results, logs, and tests |
| Claims real-use validation after setup failed | Report the exact blocker, missing configuration, errors, attempted scope, and unverified areas |
| Uses generic Issue/PR/commit text while ignoring repository conventions | Follow repository templates and contribution rules and record compliance |
| Copies a recent PR or Issue format over official rules | Apply official templates first; use recent accepted examples only for gaps |
| Uses an old, unmerged, or merely closed example as style authority | Prefer maintainer-accepted examples from the last 15 days matched by change type, module, scope, and risk |
| Deletes `.env` only in the latest commit | Revoke or rotate credentials and clean affected history and artifacts |
| Merges while CI or review is blocked | Fix the cause and wait for required gates |
| Says “done” without tests or a PR link | Report evidence and keep blocked items visible |

## Source Note

This workflow incorporates the archived Notion note `2026-08-19-notion-github-pr.md` from the Flandern vault: fork, clone, create a branch, and synchronize the fork with the original repository before pushing or opening a PR. The additional checks are deliberate completion and safety requirements for repeatable contributions.
