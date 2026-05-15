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
- Project memory index: `project_code/documentation/project_memory.md`.
- Source inventory and fidelity contract:
  `project_code/documentation/source_inventory.md`.
- Documentation outputs: `project_code/documentation/`.
- Generated app code:
  - Web app (fullstack or SPA): `project_code/app/`. This holds the entire
    Next.js/Remix/SvelteKit/Nuxt project (UI + API routes if any), or a
    standalone React/Vue SPA when the API is split out.
  - Separate web API (only when the API is a distinct Node service such as
    Express or NestJS, not framework-embedded routes): `project_code/api/`.
  - Mobile: `project_code/mobile/`.
- Environment values: root `.env`, copied from root `.env.example`.
- MCP configuration: root `.mcp.json`.

Agents must not create `app/`, `api/`, `mobile/`, `backend/`, or `frontend/` at
repository root. Prefer `app/` over the legacy `backend/`+`frontend/` split for
fullstack frameworks; the split is only for genuinely separate Node API +
SPA stacks.

## 3. Execution Policy

Within the approved project scope, implementation agents should work
autonomously until their phase is complete.

### Autonomy Mode

The `Autonomy mode` field in
`project_setup/step_1_project/step_1_project.md` controls how often Team Lead
and agents interrupt the user.

- `autonomous` (default): Team Lead runs Phase 0 through Phase 5 without
  check-ins. Agents document `N/A` decisions using `agent_team/defaults.md`
  instead of asking. The only required user touchpoints between kickoff and
  deployment are:
  1. Phase 0a Input Echo-back (single confirmation message).
  2. Phase 5 Interim Gate (local acceptance + deployment approval).
  3. Any unresolvable `[PLACEHOLDER]` values, batched into a single message
     when the upcoming phases actually need them.
- `ask`: Team Lead may stop at phase boundaries when inputs are ambiguous.

Both modes preserve hard safety gates: explicit deployment approval, critical
QA bug blocks, redeploy approval when `deployment.md` exists, and confirmation
before destructive Git or database operations.

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

### Phase 0 - Source Intake And Build Contract

Team Lead creates `agent_team/task_board.md` and writes
`project_code/documentation/source_inventory.md` before spawning PO.

Team Lead reads:

- `project_setup/step_1_project/step_1_project.md`.
- Every supported file in `project_setup/step_2_requirements/`.
- Every supported file in `project_setup/step_3_design/`.
- Any reference website URLs listed in the project spec or requirement/design
  inputs.

Input validation (do BEFORE writing source_inventory):

- Project spec validation: confirm `Name` is not the template literal
  `[PROJECT NAME]` and `Project description` is not the template literal
  `[2-4 sentences ...]`. These two are the minimum required fields and cannot
  be defaulted from `agent_team/defaults.md`. If either is still the
  template, list as a blocker in `source_inventory.md` `## Open Questions`
  and surface it in Phase 0a echo-back so the user fills it before pipeline
  continues.
- File format validation: enumerate every file under
  `project_setup/step_2_requirements/` and `project_setup/step_3_design/`. For
  each unsupported format (e.g. `.docx`, `.pptx`, `.zip`, `.key`), do NOT
  silently skip. List the file in `source_inventory.md` under
  `## Unsupported Inputs` with the recommended action (e.g. "convert to PDF
  or Markdown") and surface in Phase 0a echo-back. The user can then convert
  and rerun, or accept the gap.
- Missing-reference validation: when `step_1_project.md` or any read file
  references another file by path (e.g. "see attachment X"), confirm X exists
  on disk. List missing references in `source_inventory.md` `## Open
  Questions`.
- Empty-input validation: when both `step_2_requirements/` and
  `step_3_design/` are empty AND no reference website URL is provided, note
  this in `source_inventory.md` and proceed from project spec alone. PO will
  add `## Assumptions` to compensate.

Output:

- `project_code/documentation/source_inventory.md`.

The source inventory must include:

- Source IDs for every input, such as `SRC-PROJECT`, `REQ-001`, `DES-001`, and
  `WEB-001`.
- Source priority and conflict policy:
  - Direct user instruction in the active prompt wins.
  - Project spec and PRD define product scope.
  - Design files define visual implementation when UI is in scope.
  - Reference websites define UI/interaction/copy only to the fidelity level
    stated by the user; they do not silently expand product scope.
- Selected release scope, if already explicit; otherwise note that PO must
  recommend it.
- Requirement and design coverage checklist: screens, flows, roles, states,
  integrations, data, and acceptance checks mentioned by any source.
- Website capture notes when a URL is provided:
  - URL, pages/routes inspected, capture date, and viewport sizes used.
  - Desktop and mobile screenshot paths when capture is technically available.
  - Extracted visible copy, major UI regions, interactions, responsive behavior,
    colors, typography, spacing, and assets that appear required.
  - Any blocked access, login wall, dynamic content, cookie banner, rate limit,
    network failure, or legal/asset-use assumption.
- Concrete website capture procedure (do not silently skip a URL):
  1. Try Playwright headless first:
     `npx -y playwright install chromium` then
     `npx -y playwright screenshot --viewport-size=1440,900 --full-page <url> project_code/documentation/source_assets/web_<slug>_desktop.png`
     and again with `--viewport-size=390,844 ... _mobile.png`. Save raw HTML
     with the same tool when needed for copy extraction.
  2. If Playwright is unavailable: `curl -L --compressed <url> -o project_code/documentation/source_assets/web_<slug>.html`
     and parse visible text from the HTML.
  3. If both fail (auth wall, dynamic JS, blocked, network): record the exact
     failure mode, viewport tried, and what extra access the user could provide.
     Continue with whatever inputs are available; do not silently drop the URL.
  4. Save artifacts under `project_code/documentation/source_assets/`. Cache
     between runs; recapture only on user request.
  5. Record artifact paths and capture date in the Website Capture Notes table
     of `source_inventory.md`.
- UI image notes when PNG/JPG/WebP/PDF design inputs exist:
  - File path, screen name, image dimensions/page number, intended viewport, and
    mapped user stories.
  - Exact visible copy, visual hierarchy, reusable components, states implied by
    the image, and unknown states that Designer must define.
- Fidelity targets:
  - `Exact`: copy, layout, component states, colors, spacing, and responsive
    behavior should match the approved source as closely as implementation
    allows.
  - `Close`: preserve structure and visual feel while allowing framework-native
    details.
  - `Inspired`: use the source only for direction.
- Asset policy:
  - Use user-provided assets directly.
  - For third-party website references, do not copy proprietary logos, photos,
    or trademarked assets unless the user states they own or are allowed to use
    them. Recreate layout/style and use safe placeholders or generated assets
    when ownership is unclear.
- Open questions only for blockers or conflicts that would materially change
  scope, legal/asset risk, data model, payment-bearing services, compliance, or
  deployment. Otherwise, make an assumption, label it, and continue.

Downstream agents must read `source_inventory.md` and include a coverage or
traceability section in their deliverables showing which source IDs they covered
and what was intentionally deferred.

### Phase 0a - Input Echo-back And Autonomy Confirmation

After writing `source_inventory.md` and before spawning PO, Team Lead presents
ONE summary message to the user. This is the cheapest place to catch a
misunderstanding before the long pipeline runs.

The summary must contain:

- **Project understanding:** 1-2 sentences describing what the team will build
  and for whom, paraphrased from project spec and PRD.
- **Input completeness check (mandatory):**
  - Files found in `step_2_requirements/` with status per file: `read OK` /
    `unsupported format - convert to X` / `unreadable - <reason>`.
  - Files found in `step_3_design/` with status per file (same scheme).
  - Reference URLs with capture status: `captured (desktop+mobile)` /
    `partial - <reason>` / `blocked - <reason>`.
  - `step_1_project.md` template-literal check: any field still showing
    `[PROJECT NAME]`, `[2-4 sentences ...]`, or other template placeholders.
  - Files referenced in `step_1` or PRD that are missing from disk.
  - Total inputs vs total source IDs assigned (sanity check).
- **Inputs detected:** counts and IDs for PRD files, UI images/PDFs, reference
  URLs, Figma frames, plus per-source fidelity targets and asset policy.
- **Selected MVP 1 scope:** ordered story-shape list inferred from inputs, with
  later-MVP items deferred and reason.
- **Critical assumptions (P0 only):** decisions that, if wrong, would change
  the whole pipeline (auth model, single vs multi tenant, payments in scope,
  data sensitivity, target geography/locale, etc.).
- **Smart-default decisions taken now:** stack/framework/database/auth choices
  picked from `agent_team/defaults.md` for fields the user left as `N/A`.
- **Ambiguities worth resolving NOW:** conflicts between sources or missing
  info that would block downstream agents. Be surgical: only list items that
  actually block, not nice-to-haves.
- **`[PLACEHOLDER]` batch:** every placeholder needed for the upcoming Phase
  1-5 scope, in one list. Ask all at once instead of per-phase interruptions.
- **Autonomy plan:** when `Autonomy mode = autonomous`, state explicitly
  "I will run end-to-end and stop next at the Phase 5 interim gate."

Then stop and wait for one of:

- `go` (or equivalent confirmation): proceed end-to-end through Phase 5.
- A correction or additional info: incorporate, rewrite the affected sections
  of `source_inventory.md`, then re-present a fresh summary.

After confirmation, Team Lead must not interrupt the user for routine choices
until the Phase 5 interim gate.

### Phase 1 - Requirements

Team Lead spawns PO Agent.

PO Agent reads:

- `project_code/documentation/source_inventory.md`.
- `project_setup/step_1_project/step_1_project.md`.
- Every supported file in `project_setup/step_2_requirements/`.
- Every supported UI/reference file in `project_setup/step_3_design/` when it
  contains visible product behavior, navigation, copy, or states not already in
  requirements.

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

PO must include a `## Source Coverage` matrix mapping source IDs from
`source_inventory.md` to user stories, assumptions, or deferred items. Any
visible UI behavior found only in a PNG/PDF/reference website should become a
story, acceptance criterion, assumption, or explicit out-of-scope note.

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

Designer must include a `## Visual Parity Contract` in `design_spec.md` when UI
PNGs, PDFs, Figma frames, or reference websites are provided. It defines required
screens, viewports, states, exact copy, tokens, spacing/layout constraints,
assets, allowed substitutions, and visual tolerances for DEV/Flutter and QA.

Because this phase is parallel, the Team Lead serializes all task-board updates
after both agents complete. Parallel agents should not overwrite
`agent_team/task_board.md`.

### Phase 3 - Implementation

The Team Lead chooses the path from the Tech Stack section of the project spec:

- Web only: DEV Agent builds `project_code/app/` (and `project_code/api/` only
  when the API is a separate Node service).
- Mobile only: Flutter Agent builds `project_code/mobile/`.
- Web and mobile: DEV Agent runs first, then Flutter Agent consumes the same API
  contract and backend/client patterns.

Database setup:

- If the project uses Supabase and `SUPABASE_ACCESS_TOKEN` plus
  `SUPABASE_PROJECT_REF` are available, TechLead/DEV may use the Supabase MCP to
  inspect schema, create migrations, apply migrations to the configured Supabase
  project, and seed development/test data when needed.
- DEV must write migration files or schema setup code under `project_code/` and
  record applied migration names, commands/tools used, seed source file paths,
  seed/reset commands, seed data behavior, target database, and results in
  `dev_handoff.md`.
- Supabase seed/test data for DEV, QA, and user acceptance must target the
  configured Supabase dev/local QA project from `SUPABASE_PROJECT_REF`, not a
  production project. The actual data lives in that Supabase dev database; the
  reproducible seed source and command must live under `project_code/`,
  preferably under `project_code/app/` (or `project_code/api/` when split).
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
- `project_code/documentation/test_data.md` when auth/login or persistent
  user data is in scope. Single canonical reference for test accounts,
  passwords, sample records, suggested acceptance flows, and seed/reset
  command. DEV writes it; QA verifies it; Team Lead points to it from
  `interim_report.md` instead of duplicating credentials.

The handoff must include local install, run, test, and build commands, verification
results, required env var names, known limitations, and blockers.

When UI is in scope, DEV/Flutter must also include:

- `## Implementation Coverage` mapping user stories, API contracts, design
  screens, and source IDs to implemented files and tests.
- Screenshot or visual sanity evidence for the viewports and states required by
  Designer's Visual Parity Contract when local tooling supports it.
- Any intentional visual difference from the source, with the reason and owner
  needed to approve it.

### Phase 3.5 - Code Review

Code Review Agent runs after DEV and/or Flutter when application code changed.
Skip only when no application code changed, such as a docs-only update.

Output:

- `project_code/documentation/code_review.md`.

Code Review checks implementation against user stories, API contract, technical
design, design spec, env-boundary rules, database/migration safety, and handoff
readiness. It does not edit code.

Code Review also checks source coverage and visual parity readiness:

- All P0 source IDs from `source_inventory.md` are covered by stories, design,
  implementation, tests, or explicit deferral.
- Implemented UI follows the Visual Parity Contract for exact copy, layout,
  states, responsive behavior, and asset policy.
- DEV/Flutter handoff includes enough run commands and screenshot/verification
  evidence for QA to reproduce the visual checks.

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

For UI work, QA must run a visual parity pass against
`source_inventory.md` and `design_spec.md`:

- Verify every required screen, state, and viewport from the Visual Parity
  Contract.
- Compare exact visible copy, navigation, core layout, spacing, color/token
  usage, responsive behavior, and obvious overlaps/clipping against the approved
  PNG/Figma/reference website sources.
- Capture screenshots or evidence paths when tooling supports it.
- Record any mismatch as Critical, Major, Minor, or Note based on user impact
  and the requested fidelity target.

Before E2E, QA must confirm the database target from DEV/Flutter handoff docs
and `.env`: Supabase project, local database, or mocked/test database. QA must
verify migration and seed status before testing flows that depend on persisted
data, and record the database target, migration status, seed source file path,
seed/reset command, seeded data summary, and reset or isolation steps in
`qa_report.md`.

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
- Update `project_code/documentation/project_memory.md`.

Before writing the interim report, Team Lead reports QA results back to PO Agent
so PO can update `project_code/documentation/user_stories.md` with `## Release
Status`: completed stories, failed/deferred stories, QA notes, and recommended
next MVP/module.

Before asking the user to test locally, Team Lead must confirm that
`source_inventory.md`, `user_stories.md`, `design_spec.md`, DEV/Flutter handoff,
Code Review, and QA reports agree on source coverage. If there is a gap in a P0
source or exact-fidelity visual requirement, route it back to the owning agent
before calling the release ready unless the user explicitly accepts the gap.

Before asking about deployment, Team Lead must make the project locally testable
for the user by reading DEV, Flutter, and QA handoffs and including in the
interim report:

- Local setup commands.
- Local run commands and expected URLs, ports, emulator/device requirements, or
  build artifact paths.
- Required env var names and which ones may be left as `N/A` for local testing.
- User-acceptance test data: when auth/login or persistent user data is in
  scope, point to `project_code/documentation/test_data.md` as the canonical
  source - do NOT re-list credentials inline. Mention the standard user
  account (e.g. `user1@test.local`) so the user knows where to start, and
  link the file. When no auth is in scope, list any feature-specific inputs
  inline and explicitly state that no account is required.
- Source and visual coverage summary: the PRD/design/reference website source
  IDs covered, known differences from exact sources, and where QA visual
  evidence is recorded.
- Test data storage target: for Supabase projects, state that the data is stored
  in the configured Supabase dev/local QA project from `SUPABASE_PROJECT_REF`
  and list the seed source file path under `project_code/`.
- If the completed scope needs no seed or account data, state that explicitly
  and describe the manual data the user can create during testing.
- QA assessment, known limitations, and any blockers.

If QA is `PASS` or `PASS WITH NOTES` and local run instructions are complete,
Team Lead stops and asks the user to run one local acceptance test pass. The
user must explicitly confirm deployment before Phase 6 starts. If the user finds
issues, route them through the change-request loop or a scoped fix before
deployment.

The local acceptance request must always point the user to the test data section
in `interim_report.md`. Use only non-production sample credentials or generated
local/dev seed data; never publish real secrets, production credentials, or real
user data.

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
- Update `project_code/documentation/project_memory.md`.

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
- Team Lead updates `project_code/documentation/project_memory.md` with the
  change result, linked report, and next action.
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
- Team Lead updates `project_code/documentation/project_memory.md` at major
  gates so later sessions can resume from a concise state summary.
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
| `[PLACEHOLDER]` | User must provide it | Batch with other placeholders, ask once, write answers back |
| `N/A` | User delegates the decision | Responsible agent picks from `agent_team/defaults.md` and documents it |
| Empty secret | Secret not configured yet | Ask only if the upcoming phase needs it; batch with other placeholders |

Batching rule: before spawning the first agent that needs `[PLACEHOLDER]` or
empty secret values, Team Lead scans every value needed for the upcoming
Phase X through Phase 5 scope and asks for all of them in one message.
The Phase 0a Input Echo-back is the natural place to do this for the kickoff.

Default rule: an `N/A` field is not a question. The responsible agent reads
`agent_team/defaults.md`, picks the catalog default, and records the choice in
its deliverable's `## N/A Decisions` (or equivalent) section. Only escalate to
the user when the catalog has no row that fits and the choice would change
product scope, security posture, or external accounts.

Every `N/A`-derived decision must appear in the relevant deliverable and in the
Team Lead interim/final report.
