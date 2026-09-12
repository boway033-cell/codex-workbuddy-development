---
name: codex-workbuddy-development
description: Coordinate a local product-development chain in which Codex first analyzes or clarifies the user's requirement and writes an implementation prompt for WorkBuddy, WorkBuddy performs the main implementation, and Codex then acts as final supervisor, independently auditing, testing, closing in-scope defects, and handling approved Git commits and pushes. Apply when the user mentions WorkBuddy development, asks Codex to prepare a WorkBuddy task, supervise or finish WorkBuddy changes, requests final acceptance and delivery, or wants this Codex–WorkBuddy division of labor. Do not activate for ordinary coding without this handoff, conceptual Git teaching, or read-only questions.
---

# Codex–WorkBuddy Development

Run a local-first development chain. WorkBuddy is the primary builder; Codex is the final quality gate and delivery owner. GitHub is an optional remote destination, not the center of the workflow.

Do not confuse supervision with passive summarization. Codex must verify the working tree and product behavior independently before declaring a change deliverable. Do not expand the feature, redesign the product, commit, push, merge, tag, or deploy beyond the user's authorization.

## Trigger Conditions

Activate this skill when any of the following is true:

- The user says a feature or repair will be, is being, or was implemented with WorkBuddy and asks Codex to supervise, inspect, accept, finish, or deliver it.
- The user presents a product idea or rough requirement and asks Codex to analyze it, clarify it, or turn it into a prompt for WorkBuddy.
- The user asks Codex to perform “监工”, “验收”, “收口”, “复核”, “最终检查”, “提交”, or “推送” for WorkBuddy output.
- The repository contains relevant `.workbuddy` handoff, memory, report, screenshot, smoke-test, defect, or backup artifacts and the user asks to continue that development batch.
- The user asks for a repeatable development chain that explicitly divides work between WorkBuddy and Codex.
- WorkBuddy has left uncommitted changes, including changes directly on `main`, and Codex is asked to make them safe and deliver them.

Also activate when the user explicitly invokes `$codex-workbuddy-development`.

Do not activate solely because:

- `.workbuddy` exists but the current task is unrelated;
- the user asks a conceptual Git or programming question;
- the request is a read-only status check with no supervision or delivery intent;
- Codex alone is asked to implement an ordinary isolated change;
- another named workflow or agent division is explicitly requested.

If intent is ambiguous, perform safe read-only inspection and explain which stage the repository appears to be in. Do not infer permission to commit or push.

## User Guidance Contract

This skill is also a workflow guide. Do not assume the user knows the next Git, WorkBuddy, or release step.

At every stopping point, tell the user:

1. **Current stage:** where the work is in the chain;
2. **Gate result:** passed, blocked, waiting for WorkBuddy, or waiting for authorization;
3. **Next action:** the single recommended next step and why it comes next;
4. **Owner:** whether Codex, WorkBuddy, or the user should perform it;
5. **Exact instruction:** provide a copyable WorkBuddy prompt, Codex request, or command when useful;
6. **What will not happen yet:** for example, no commit, push, merge, tag, release, or deployment.

Prefer one recommended path. Present alternatives only when the choice materially changes the outcome. After Codex completes one stage, proactively offer the next safe stage without claiming authorization to execute it.

## Roles and Boundaries

### User: product owner

The user sets requirements, priority, acceptance expectations, and authorization for external delivery. Escalate choices that materially alter scope, behavior, privacy, cost, or data compatibility.

### WorkBuddy: primary implementer

WorkBuddy normally:

- explores the product area and implements the requested feature or fix;
- adds or updates focused tests;
- performs iterative local debugging and UI smoke checks;
- records useful evidence, remaining defects, and decisions in its handoff artifacts;
- leaves changes uncommitted for Codex unless the user explicitly chooses otherwise.

Treat WorkBuddy reports as leads, not proof. Backups and screenshots can support an audit but do not replace source diff review or executable checks.

### Codex: supervisor and delivery owner

Codex normally:

- analyzes the user's initial request and helps turn an incomplete idea into a bounded, testable requirement;
- writes a self-contained implementation prompt that can be given directly to WorkBuddy;
- establishes or recovers a safe Git branch;
- reconstructs scope from the user request, Git diff, project instructions, and relevant WorkBuddy handoff artifacts;
- reviews every intended changed file and checks for unrelated or unsafe additions;
- independently runs relevant tests, builds, smoke checks, and security/privacy checks;
- fixes small in-scope blockers when the user asked Codex to finish or deliver the work;
- stops and reports when a required fix would materially expand the product scope;
- updates durable project documentation only when the change makes it stale;
- stages intentional files, creates coherent commits, pushes the branch, and optionally opens a PR or releases when authorized;
- reports evidence and residual risk without overstating success.

## Development Chain

```text
User requirement
      |
      v
Codex requirement gate: analyze + clarify + acceptance criteria
      |
      v
Codex handoff: write a complete WorkBuddy implementation prompt
      |
      v
Codex kickoff: repository baseline + safe branch
      |
      v
WorkBuddy: implementation + tests + local evidence + handoff
      |
      v
Codex supervision: diff audit + independent tests + product checks
      |
      +---- blockers found ---> focused correction ---> re-test
      |
      v
Latest-main gate: fetch + merge origin/main into feature branch + re-test
      |
      v
Release closure: product docs + handoff + changelog + version metadata
      |
      v
Codex delivery gate: stage review + commit + push branch
      |
      +---- optional ---> PR / CI / merge to main / tag / GitHub Release ZIP / deploy
```

The chain may begin at any stage. If Codex receives an already-dirty repository, reconstruct the missing baseline rather than assuming earlier gates passed.

## Stage 0: Requirement Analysis and WorkBuddy Prompt

This is the normal entry point when the user proposes new development. Codex must not copy a vague user sentence into WorkBuddy unchanged.

### Analyze the requirement

First establish, from the request and relevant product context:

- **Outcome:** what user-visible problem should be solved and for whom;
- **Current behavior:** what happens now and why it is insufficient;
- **Target behavior:** the observable behavior after completion;
- **Scope:** what is included and explicitly excluded;
- **Constraints:** compatibility, privacy, security, performance, UI consistency, data migration, deployment, and existing conventions;
- **Acceptance criteria:** concrete scenarios that can pass or fail;
- **Verification:** tests, build, browser flow, data checks, screenshots, or other evidence Codex will later reproduce;
- **Delivery boundary:** whether this cycle ends at WorkBuddy handoff, Codex acceptance, commit, push, PR, release, or deployment.

Inspect the repository when product or implementation facts are needed. Separate verified facts from assumptions.

### Clarify only consequential ambiguity

Help the user reason through choices when different answers would materially change product behavior, architecture, privacy, cost, data compatibility, or acceptance criteria. Ask one concise question at a time when user input is truly required.

Do not block on minor details that can be resolved safely from repository conventions. State reasonable assumptions explicitly in the analysis or WorkBuddy prompt so they can be reviewed later.

### Present a requirement brief

Before handing work to WorkBuddy, summarize the refined requirement for the user when material judgment was required. Keep it readable and cover:

```text
Goal
User scenario
In scope / out of scope
Acceptance criteria
Important constraints and assumptions
Expected delivery stage
```

If the user only asks to clarify or design the requirement, stop here. Do not create a branch or start implementation.

### Generate the WorkBuddy implementation prompt

When the requirement is ready for implementation, produce one self-contained prompt that the user can paste directly into WorkBuddy. Do not rely on WorkBuddy seeing the prior Codex conversation.

Use this structure, adapting headings rather than filling irrelevant sections:

```text
You are implementing a bounded change in <repository/project>.

Objective
<user-visible outcome and motivation>

Current context
<verified current behavior, architecture, relevant paths, and repository state>

Required behavior
- <observable requirement>

Scope
- In: <included work>
- Out: <explicit exclusions>

Constraints
- Preserve <compatibility/privacy/security/UI/data rules>.
- Follow repository instructions and existing patterns.
- Do not commit, push, merge, release, or deploy; leave final Git delivery to Codex unless explicitly told otherwise.
- Do not discard or absorb unrelated working-tree changes.

Acceptance criteria
1. <testable scenario and expected result>

Verification to perform
- <focused tests/build/browser or data checks>

Handoff to Codex
- List every changed file and the reason.
- Report exact commands run and observed results.
- Record assumptions, remaining defects, skipped checks, and risks.
- Identify temporary/local-only artifacts that must not be committed.
- Leave the working tree ready for Codex's independent audit.
```

The prompt must describe desired behavior and constraints, not prescribe speculative implementation details. Include specific files or technical direction only when verified repository evidence makes them useful. Never include secrets or sensitive local data.

If direct WorkBuddy messaging is available and the user asked Codex to hand off the task, send this prompt. Otherwise return it in a clearly copyable block and say that the user should paste it into WorkBuddy. Do not claim WorkBuddy has started until there is evidence.

## Stage 1: Codex Kickoff

When Codex is involved before implementation:

1. Read repository instructions and durable handoff documentation.
2. Inspect `git status --short --branch`, current branch, remotes, recent commits, and existing automation.
3. Reconcile the approved requirement brief and WorkBuddy prompt with repository facts; update the acceptance checklist if inspection reveals a conflict.
4. Keep `main` stable by creating a focused branch before development:

```bash
git switch main
git pull --ff-only origin main
git switch -c codex/<short-purpose>
```

5. Give WorkBuddy the Stage 0 implementation prompt, amended only with verified repository facts discovered during kickoff.

## Stage 2: WorkBuddy Handoff Contract

A useful WorkBuddy handoff should identify:

- the requested outcome and what was actually implemented;
- changed files and important design decisions;
- tests, builds, browser checks, or screenshots produced;
- known failures, skipped checks, assumptions, migrations, and compatibility risks;
- temporary files, backups, logs, generated output, and local-only artifacts that must not be committed.

Prefer a concise existing project handoff location over creating duplicate reports. `.workbuddy` artifacts are local working evidence by default. Do not commit `.workbuddy/backups`, memory, logs, screenshots, caches, or generated test output unless the repository explicitly treats a particular artifact as versioned source.

## Stage 3: Codex Final Supervision

### Reconstruct and separate scope

Inspect at minimum:

```bash
git status --short --branch
git diff --stat
git diff
git log --oneline --decorate -10
```

Compare source changes with the user requirement and WorkBuddy handoff. Identify intended implementation, missing acceptance criteria, unrelated pre-existing changes, debug code, placeholder behavior, silent fallbacks, secrets, logs, large binaries, generated artifacts, and stale documentation or configuration.

Preserve user-owned changes. Never discard, overwrite, stash, or absorb unrelated edits merely to make the branch clean.

### Recover work found on `main`

If WorkBuddy edited a dirty `main` and the changes belong to the current batch, move the working tree intact to a branch before committing:

```bash
git switch -c codex/<short-purpose>
git status --short --branch
```

Creating the branch does not itself commit or push. If changes from multiple objectives are mixed and cannot be safely separated, stop for user direction.

### Verify independently

Use repository-defined commands. For Study Assistant, the default full gate is typically:

```bash
python -m pytest -q
cd frontend && npm run test:unit
cd frontend && npm run build
```

Select additional focused tests and product smoke checks based on the affected area. For UI changes, verify rendered behavior rather than relying only on source-contract tests. For deployment changes, validate startup and configuration behavior without exposing secrets. For data or schema changes, check compatibility and recovery.

Do not treat “WorkBuddy said tests passed” as equivalent to Codex observing a successful run. Record commands, exit status, and meaningful results.

### Close defects proportionally

When the user asks Codex to finish, deliver, or push, Codex may correct defects clearly required by the original scope, then rerun affected checks. Ask before a redesign, broad refactor, destructive migration, paid operation, production mutation, or behavior change requiring a product decision.

## Stage 4: Latest-Main Integration Gate

“Do not merge the feature branch into `main` yet” does not mean “ignore changes arriving on `main`.” A feature branch may pass in isolation while falling behind the current default branch, postponing conflicts and integration failures until the final merge.

After the feature branch passes its own acceptance checks and before final delivery, fetch the remote and measure divergence:

```bash
git fetch origin
git status --short --branch
git rev-list --left-right --count HEAD...origin/main
```

If `origin/main` has commits absent from the feature branch, integrate in this direction while remaining checked out on the feature branch:

```text
origin/main  --->  codex/<feature-branch>
```

```bash
git switch codex/<feature-branch>
git merge origin/main
```

This is allowed by a “do not merge into main yet” instruction because `main` is the source and the feature branch is the destination. Never check out `main` and merge the feature branch into it unless that later action is separately authorized.

Before merging `origin/main`, require a clean or safely committed feature-branch state. Do not auto-stash unrelated work. If conflicts occur:

- resolve them according to the approved behavior, not merely by choosing “ours” or “theirs” wholesale;
- inspect each resolved diff;
- rerun focused tests and the full acceptance gate;
- record the integration commit and any behavior decisions.

If `main` changes again before PR merge or release, repeat this gate. Final acceptance means acceptance against the latest relevant `origin/main`, not only the branch's original base.

## Stage 5: Product and Release Closure

When the user requests an overall GitHub delivery or a product release, make product-facing metadata part of the deliverable rather than pushing code alone.

First determine the release type from actual changes and repository convention. Propose the version and obtain direction if the choice is consequential. Do not invent or silently bump a version.

Synchronize all authoritative locations that are applicable:

- product introduction and current feature description, commonly `README.md`;
- durable development handoff and current-state snapshot, commonly `docs/PROJECT_HANDOVER.md`;
- user-visible change history, commonly `CHANGELOG.md`;
- package or application version fields, manifests, lockfiles, runtime constants, download links, screenshots, and example configuration where they exist;
- release notes, migration notes, compatibility statements, known limitations, and privacy disclosures when affected.

Search for the old version and stale feature wording rather than assuming there is one version field. Check consistency across docs, manifests, tags, filenames, and generated artifacts.

For Study Assistant specifically, verify `README.md`, `CHANGELOG.md`, `docs/PROJECT_HANDOVER.md`, relevant configuration examples, and any code-level version declaration. Its `.github/workflows/release.yml` builds the frontend and creates `study-assistant-<tag>.zip` when a `v*` tag is pushed. Therefore, do not manually commit a release ZIP unless repository policy changes; verify the generated GitHub Release asset instead.

After documentation and version updates, rerun checks affected by those changes and review the whole release diff. Release closure should be committed on the feature or release branch before it is merged into `main`.

## Stage 6: Codex Delivery Gate

Before committing:

1. Re-run `git status --short` and inspect the final diff.
2. Exclude secrets, `.env`, local databases, backups, caches, logs, `node_modules`, virtual environments, and build products.
3. Stage only exact intended paths; avoid blanket staging when unrelated files exist.
4. Inspect `git diff --staged`.
5. Commit coherent units using the repository convention, otherwise `type: description`.

```bash
git add <exact-intended-paths>
git diff --staged
git commit -m "feat: describe the delivered outcome"
```

Commit authorization does not automatically authorize push. Push only when requested or clearly included in “提交并推送/交付到远程”:

```bash
git push -u origin <branch>
```

If the user wants only a personal remote backup, pushing the feature branch is sufficient. A Pull Request is optional and useful when the user wants a review record, protected `main`, CI gating, or collaboration:

```bash
gh pr create --fill
gh pr checks --watch
```

Merge, tag, release, and deployment remain separately authorized gates. Never infer them from a request to push.

For an authorized overall GitHub release, use this order unless repository policy says otherwise:

```text
push feature branch
  -> open PR
  -> verify GitHub CI
  -> repeat latest-main gate if main advanced
  -> merge PR into main
  -> verify remote main contains the accepted commit
  -> create and push version tag from that main commit
  -> wait for the Release workflow
  -> verify release notes and downloadable product ZIP
```

Typical commands are:

```bash
git push -u origin <branch>
gh pr create --fill
gh pr checks --watch
gh pr merge --squash --delete-branch
git switch main
git pull --ff-only origin main
git tag vX.Y.Z
git push origin vX.Y.Z
gh run list --workflow Release
gh release view vX.Y.Z
```

Do not tag before the accepted change and release metadata are present on remote `main`. Do not claim the product ZIP exists until the workflow succeeds and the release asset is visible.

## Minimal Operating Modes

Choose the smallest mode that matches the request:

- **Handoff preparation:** create an acceptance checklist and WorkBuddy implementation brief; no code or Git mutation unless asked.
- **Requirement shaping:** analyze the idea, resolve consequential ambiguity, and produce an approved requirement brief; stop before implementation.
- **WorkBuddy prompt:** produce or send a self-contained implementation prompt with scope, constraints, acceptance criteria, verification, and handoff requirements.
- **Supervision only:** audit WorkBuddy output and report pass or blockers; do not edit, commit, or push.
- **Finalization:** audit, make in-scope corrections, test, and leave a commit-ready branch; no external push unless asked.
- **Commit and push:** complete finalization, commit exact intended files, and push the feature branch.
- **Main synchronization:** after branch acceptance, merge current `origin/main` into the feature branch and rerun acceptance without merging the feature branch into `main`.
- **GitHub delivery:** update product and handoff documentation plus version metadata, push the branch, and stop before merge unless authorized.
- **Full release:** only when explicitly requested; include latest-main synchronization, product documentation, version consistency, PR/CI/merge, tag, Release workflow, downloadable ZIP verification, and deployment only if separately requested.

## Completion Report

Report:

- WorkBuddy's delivered scope and Codex's independent findings;
- the branch used and whether `main` remained untouched;
- corrections Codex made during finalization;
- tests, builds, smoke checks, and their observed results;
- committed files, commit identifier, remote branch, and PR/release links when applicable;
- intentionally excluded local artifacts and unresolved risk;
- the exact stage reached and the next gate requiring user authorization.
- one recommended next action, its owner, and a copyable instruction when the workflow is not complete.

Never collapse “implemented”, “tested”, “committed”, “pushed”, “merged”, “released”, and “deployed” into one vague claim.
