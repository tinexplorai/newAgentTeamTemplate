# Agent Team - Workflow

This file is framework-level. Project-specific context lives in
`project_setup/step_1_project/step_1_project.md`.

## 1. Agent Team

- Team Lead / Orchestrator - main Claude Code session. Owns the task board,
  phase sequencing, gates, and reports.
- PO Agent - `agents/Agent_01_PO.md`.
- TechLead Agent - `agents/Agent_02_TechLead.md`.
- Designer Agent - `agents/Agent_03_Designer.md`. Skip for backend-only work.
- DEV Agent - `agents/Agent_04_DEV.md`. Skip for mobile-only work.
- Flutter Agent - `agents/Agent_05_Flutter.md`. Run only when mobile is in
  scope.
- Code Review Agent - `agents/Agent_08_CodeReview.md`. Runs after
  implementation when application code changed.
- QA Agent - `agents/Agent_06_QA.md`.
- DevOps Agent - `agents/Agent_07_DevOps.md`. Run only after explicit user
  approval.

Model assignments live in `agent_team/agents_config.md`.

## 2. Canonical Paths

- Project spec: `project_setup/step_1_project/step_1_project.md`.
- Optional requirements: `project_setup/step_2_requirements/`.
- Optional design inputs: `project_setup/step_3_design/`.
- Task board: `agent_team/task_board.md`.
- Documentation outputs: `project_code/documentation/`.
- Generated app code:
  - Web backend: `project_code/backend/`.
  - Web frontend: `project_code/frontend/`.
  - Mobile: `project_code/mobile/`.
- Environment values: root `.env`, copied from root `.env.example`.
- MCP configuration: root `.mcp.json`.

Agents must not create `backend/`, `frontend/`, or `mobile/` at repository root.

## 3. Execution Policy

Within the approved project scope, implementation agents should work
autonomously until their phase is complete.

- DEV and Flutter Agents may create, edit, install dependencies for, run, test,
  debug, and refactor generated application code under `project_code/` without
  asking for additional approval for routine implementation choices.
- QA Agent does not edit production code. QA owns verification, triage,
  evidence, owner recommendation, and reruns after the assigned owner fixes the
  issue.
- Code Review Agent does not edit production code. It reviews implementation
  quality and readiness, recommends owners for findings, and blocks QA when
  critical issues remain.
- After explicit deployment approval at Phase 5, DevOps Agent may push to Git,
  create or update CI workflows, add smoke tests, configure Vercel, and trigger
  production deploy without asking for per-step approval.
- Agents should stop and escalate to Team Lead only when the next step would
  change product scope, API contract, data schema, architecture, external
  accounts/projects, deployment before Phase 5 approval, payment-bearing
  services, secrets, signing material, or destructive Git history.
- Agents must never print secret values into docs, logs, reports, or the task
  board. Use environment variable names only.
- Team Lead should avoid interrupting implementation agents for routine progress
  unless they are blocked, but must record each handoff in the task board when an
  agent finishes.

## 4. Process

### Phase 1 - Requirements

Team Lead creates `agent_team/task_board.md` and spawns PO Agent.

PO Agent reads:

- `project_setup/step_1_project/step_1_project.md`.
- Every supported file in `project_setup/step_2_requirements/`.

Output:

- `project_code/documentation/user_stories.md`.

PO must include a release plan when the project is larger than one small release:

- `MVP 1`: the smallest local-testable and deployable release.
- `MVP 2`: important follow-up features.
- `Later`: deferred features.

Each story should include priority (`P0`, `P1`, `P2`) and dependencies. Team Lead
records the selected release scope in `agent_team/task_board.md`. Downstream
agents build only the selected release scope unless the user explicitly expands
scope.

TechLead uses the full release plan for product-wide architecture and data-model
direction, but writes detailed implementation contracts only for the selected
release. Later-release endpoints, schema, and integrations should be documented
as roadmap notes when they affect compatibility, not implemented until selected.

### Phase 2 - Design And Architecture

Team Lead spawns TechLead Agent and Designer Agent in parallel, unless the
project is backend-only.

TechLead output:

- `project_code/documentation/api_contract.md`.
- `project_code/documentation/tech_design.md` only when non-trivial.

Designer output:

- `project_code/documentation/design_spec.md`.
- Optional assets under `project_code/documentation/design_assets/`.

Because this phase is parallel, the Team Lead serializes all task-board updates
after both agents complete. Parallel agents should not overwrite
`agent_team/task_board.md`.

### Phase 3 - Implementation

The Team Lead chooses the path from the Tech Stack section of the project spec:

- Web only: DEV Agent builds `project_code/backend/` and/or
  `project_code/frontend/`.
- Mobile only: Flutter Agent builds `project_code/mobile/`.
- Web and mobile: DEV Agent runs first, then Flutter Agent consumes the same API
  contract and backend/client patterns.

Database setup:

- If the project uses Supabase and `SUPABASE_ACCESS_TOKEN` plus
  `SUPABASE_PROJECT_REF` are available, TechLead/DEV may use the Supabase MCP to
  inspect schema, create migrations, apply migrations to the configured Supabase
  project, and seed development/test data when needed.
- DEV must write migration files or schema setup code under `project_code/` and
  record applied migration names, commands/tools used, seed data behavior, and
  results in `dev_handoff.md`.
- If Supabase values are missing or `[PLACEHOLDER]`, DEV should create the
  migration files locally, document the blocker, and ask Team Lead to get the
  missing values once.
- If the project uses a local database such as SQLite or local PostgreSQL, DEV
  should create local setup/migration/seed commands and document them in
  `dev_handoff.md`.
- Stop and escalate before applying destructive migrations, deleting data,
  changing schema outside `api_contract.md`/`tech_design.md`, or touching a
  production database.

Output:

- `project_code/documentation/dev_handoff.md` when DEV runs.
- `project_code/documentation/flutter_handoff.md` when Flutter runs.

The handoff must include local install, run, test, and build commands, verification
results, required env var names, known limitations, and blockers.

### Phase 3.5 - Code Review

Code Review Agent runs after DEV and/or Flutter when application code changed.
Skip only when no application code changed, such as a docs-only update.

Output:

- `project_code/documentation/code_review.md`.

Code Review checks implementation against user stories, API contract, technical
design, design spec, env-boundary rules, database/migration safety, and handoff
readiness. It does not edit code.

If Code Review finds failures, it records evidence, severity, likely owner, and
recommended handoff in `code_review.md` and returns to Team Lead. Team Lead
assigns the fix to the appropriate owner. After the owner fixes the issue and
updates its handoff, Team Lead may rerun Code Review for the affected area before
starting QA.

Critical findings block QA. Major findings usually block QA unless Team Lead
explicitly accepts them as notes with clear user impact.

### Phase 4 - QA

QA Agent runs local tests for every generated target:

- Backend unit/integration tests.
- Web frontend unit and Playwright E2E tests.
- Flutter unit/widget and `integration_test` tests.

Before E2E, QA must confirm the database target from DEV/Flutter handoff docs
and `.env`: Supabase project, local database, or mocked/test database. QA must
verify migration and seed status before testing flows that depend on persisted
data, and record the database target, migration status, seed data, and reset or
isolation steps in `qa_report.md`.

Output:

- `project_code/documentation/qa_report.md`.

If QA finds failures, QA records evidence, severity, likely owner, and
recommended handoff in `qa_report.md` and returns to Team Lead. Team Lead assigns
the fix to the appropriate owner:

- DEV for web/backend implementation, dependency, migration, seed, local-run, or
  API implementation bugs.
- Flutter for mobile implementation, widget, integration, build, or platform
  issues.
- TechLead for API contract, schema, architecture, or technical-design gaps.
- Designer for design-spec ambiguity or UI requirements gaps.
- DevOps for CI/deploy/smoke configuration issues.
- User/Team Lead for missing env values, external account permissions, or
  product-scope decisions.

After the owner fixes the issue and updates its handoff, QA reruns the failed
tests plus any affected regression scope and updates `qa_report.md`. QA must not
edit production code, schemas, API contracts, migrations, or app behavior.

### Phase 5 - Local User Acceptance And Interim Gate

Team Lead writes:

- `project_code/documentation/interim_report.md`.

Before writing the interim report, Team Lead reports QA results back to PO Agent
so PO can update `project_code/documentation/user_stories.md` with `## Release
Status`: completed stories, failed/deferred stories, QA notes, and recommended
next MVP/module.

Before asking about deployment, Team Lead must make the project locally testable
for the user by reading DEV, Flutter, and QA handoffs and including in the
interim report:

- Local setup commands.
- Local run commands and expected URLs, ports, emulator/device requirements, or
  build artifact paths.
- Required env var names and which ones may be left as `N/A` for local testing.
- Test credentials, seed data, or manual setup steps when applicable.
- QA assessment, known limitations, and any blockers.

If QA is `PASS` or `PASS WITH NOTES` and local run instructions are complete,
Team Lead stops and asks the user to run one local acceptance test pass. The
user must explicitly confirm deployment before Phase 6 starts. If the user finds
issues, route them through the change-request loop or a scoped fix before
deployment.

User acceptance bug triage:

- If the user finds a bug during local acceptance and wants QA only to reproduce
  it first, Team Lead may spawn QA Agent alone before assigning any fix owner.
- QA reads the user's reproduction steps, `interim_report.md`, `qa_report.md`,
  DEV/Flutter handoffs, and relevant docs, then runs the smallest targeted
  reproduction needed.
- QA writes
  `project_code/documentation/user_acceptance_bug_<short-title-slug>.md` with
  environment, database target, steps, expected result, actual result, evidence,
  reproduced/not reproduced status, likely owner, and recommended next action.
- If reproduced, Team Lead assigns the fix to the likely owner. After the owner
  fixes and updates its handoff, QA reruns the failed path plus affected
  regression scope.
- If not reproduced, QA records what was tested and what extra user details,
  data, browser/device, account, or env values are needed.

If QA is `FAIL` or local testing is blocked, Team Lead asks whether to fix
first, explicitly override the risk, or stop. DevOps must not run without
explicit approval.

Critical open QA issues block deployment unless the user explicitly overrides
the block in writing.

### Phase 6 - Deployment

DevOps Agent runs only after the Phase 5 approval.

Output:

- `project_code/documentation/deployment.md`.
- CI workflow files under `.github/workflows/` when applicable, including smoke
  tests for the deployed surface when technically possible. DevOps CI should not
  rerun the full local QA regression suite unless the user or project spec
  explicitly requires it.

After approval, DevOps should complete the deployment track end to end without
asking for routine per-step approval: push Git, create or update CI workflows,
add smoke tests, configure Vercel for web, trigger production deploy, observe CI
and deployment status, and write the deployment report. Full regression remains
the responsibility of QA before the deployment gate. DevOps stops only for
missing required env values, failed CI/deploy checks, external account/project
creation, consent prompts, secrets/signing material that must be provided by the
user, or destructive Git operations.

If deployment fails, DevOps classifies the failure and records deployment IDs,
log summaries, retries, and next owner in `deployment.md`:

- Deployment configuration owned by DevOps, such as Vercel root directory, build
  command, output directory, env var names, project linking, or smoke route
  configuration: DevOps fixes and retries without extra approval.
- Application code, dependency/build failure, runtime exception, API contract
  mismatch, or missing behavior: hand off to DEV or Flutter, then rerun scoped QA
  before asking the user to confirm redeploy.
- Vercel/GitHub account, token permission, paid-plan limit, consent prompt, or
  missing user-provided secret: stop and ask the user through Team Lead.

Deployment is not complete until CI, deploy, and smoke checks pass, or until the
Team Lead records an explicit written user override.

### Phase 7 - Final Report

Team Lead writes:

- `project_code/documentation/final_report.md`.

The final report includes deployment URLs, CI status, manual secrets/setup still
needed, and follow-up recommendations.

After each completed MVP or accepted local release, Team Lead prepares a release
decision summary before starting the next release. The summary should read
`user_stories.md`, `code_review.md`, `qa_report.md`, `interim_report.md`, and
`deployment.md` if present, then present:

- Current release status and user acceptance result.
- Completed, failed, deferred, and newly discovered stories.
- PO recommended next MVP/module.
- TechLead technical risks, dependencies, and architecture implications for the
  next release.
- 2-3 concrete next-build options, each with expected agents, scope, risks, and
  whether deployment approval would be needed afterward.

Team Lead must wait for the user to choose the next release/module before
running Phase 9 agents.

### Phase 9 - Change Request Loop

For changes after QA or deployment, use
`project_kickoff/2_prompt_change_request.md`.

Classify the request:

- QA-only triage: user found or suspects a bug and wants reproduction/evidence
  before assigning a fix. Run QA only.
- Small: copy, color, spacing, or a single-component bugfix. Run DEV or Flutter,
  then Code Review when application code changed, then QA.
- Medium: layout changes, new UI component, or design-token changes. Run
  Designer, DEV or Flutter, then Code Review, then QA.
- Large-backend: new endpoint, schema change, or business logic. Run PO,
  TechLead, DEV or Flutter, then Code Review, then QA.
- Large-UX: new screen or changed user journey. Run PO, TechLead, Designer, DEV
  or Flutter, then Code Review, then QA.

Rules:

- Append to existing docs instead of rewriting prior decisions.
- QA regression scope must include all flows sharing code with the change.
- Team Lead opens a new `## Phase 9 - Change Request: <short title>` section in
  `agent_team/task_board.md`.
- Team Lead writes
  `project_code/documentation/change_report_<short-title>.md` after QA passes.
- If `project_code/documentation/deployment.md` exists, redeploy requires the
  same explicit approval as the initial deployment gate.

## 5. Phase Reporting Standard

Every phase must leave a reviewable trail for the user.

- The responsible agent writes its required deliverable under
  `project_code/documentation/`.
- The agent returns a handoff summary with deliverables written, commands/checks
  run, assumptions, `N/A` decisions, blockers, and follow-up work.
- Team Lead records the handoff summary, phase status, and deliverable paths in
  `agent_team/task_board.md`.
- Team Lead reports the phase result to the user before moving through major
  gates: after requirements/design, after implementation, after QA, before
  deployment, and after deployment.

## 6. Task Board Ownership

`agent_team/task_board.md` is the shared coordination source of truth, but the
Team Lead owns writes to avoid file conflicts during parallel work.

Agents read the task board for context. At completion, each agent returns a
short handoff summary to the Team Lead including:

- Deliverables written.
- Tests or checks run.
- Assumptions and `N/A` decisions.
- Blockers or follow-up work.

The Team Lead records the status, checkboxes, and message rows in the task
board.

## 7. External Resources

Concrete identifiers and secrets live in root `.env`. Copy from `.env.example`.
MCP server definitions live in root `.mcp.json`.

Project-specific runtime secrets such as `OPENAI_API_KEY`,
`OPENROUTER_API_KEY`, email-provider keys, payment keys, or analytics keys must
be added to `.env` using clear variable names. Agents may introduce new env var
names when the approved project scope requires them, but they must document the
names, purpose, client/server boundary, and local/Vercel/CI requirements in the
relevant handoff or report. Never print secret values.

Mobile signing keys must not be stored in `.env`. DevOps documents required
GitHub Secret names in `project_code/documentation/deployment.md`.

## 8. Missing Values

Use these markers in `project_setup/step_1_project/step_1_project.md` and
`.env`:

| Marker | Meaning | Behavior |
| --- | --- | --- |
| Real value | The value is known | Use it as-is |
| `[PLACEHOLDER]` | User must provide it | Ask once, then write the answer back |
| `N/A` | User delegates the decision | Responsible agent decides and documents it |
| Empty secret | Secret not configured yet | Ask only if the current phase needs it |

Every `N/A`-derived decision must appear in the relevant deliverable and in the
Team Lead interim/final report.
