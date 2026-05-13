# SDLC Agent Team Framework

Reusable framework for building software with Claude Code Agent Teams. Each
agent owns one SDLC role, while the Team Lead orchestrates the flow through
shared files.

## Overview

This template coordinates specialist Claude agents to build complete software
projects:

- Team Lead: orchestrates phases, owns `agent_team/task_board.md`, writes reports.
- PO Agent: converts project inputs into user stories and release plans.
- TechLead Agent: writes API contracts and technical design with a product-wide
  architecture view across planned MVPs.
- Designer Agent: turns design inputs or Figma frames into implementation specs.
- DEV Agent: builds backend and web frontend code.
- Flutter Agent: builds mobile apps when mobile is in scope.
- Code Review Agent: reviews generated code and blocks QA on critical findings.
- QA Agent: runs tests, triages failures to owners, reruns after fixes, writes QA
  report.
- DevOps Agent: pushes, configures CI, and deploys only after human approval.

Generated project code and deliverables live under `project_code/`.

## Agent Team

| Agent | Default model | Role |
| --- | --- | --- |
| Team Lead | `opus` | Orchestrates workflow and reports |
| PO Agent | `opus` | Requirements and user stories |
| TechLead Agent | `opus` | API contract and architecture |
| Designer Agent | `sonnet` | UI/UX specification |
| DEV Agent | `sonnet` | Backend, web frontend, tests |
| Flutter Agent | `sonnet` | Mobile app, mobile tests |
| Code Review Agent | `sonnet` | Code review and QA readiness |
| QA Agent | `sonnet` | Test execution, triage, QA report |
| DevOps Agent | `sonnet` | GitHub, CI/CD, deploy |

Model assignments are centralized in
[`agent_team/agents_config.md`](agent_team/agents_config.md).

## Canonical Workflow

1. Phase 1 - Requirements: PO writes
   `project_code/documentation/user_stories.md` with a release plan when needed.
2. Phase 2 - Design and architecture: TechLead and Designer run in parallel.
   TechLead writes `api_contract.md` and optionally `tech_design.md`;
   Designer writes `design_spec.md`.
3. Phase 3 - Implementation: DEV builds web/backend under `project_code/`.
   Flutter builds `project_code/mobile/` when mobile is in scope.
4. Phase 3.5 - Code review: Code Review writes `code_review.md` when
   application code changed.
5. Phase 4 - QA: QA runs local automated tests and writes `qa_report.md`.
6. Phase 5 - Interim gate: PO updates release status from QA results, then Team
   Lead writes `interim_report.md` and asks the user before deployment.
7. Phase 6 - Deployment: DevOps pushes, configures CI/CD, deploys, and writes
   `deployment.md`. This phase requires explicit user approval.
8. Phase 7 - Final report: Team Lead writes `final_report.md`.
9. Next release decision: Team Lead presents 2-3 next-build options before
   starting the next module/release.

Full details are in [`agent_team/workflow.md`](agent_team/workflow.md).

## Quick Start

### 1. Prerequisites

- Claude Code CLI or VS Code extension.
- Git.
- Runtime for the project stack, such as Node.js, Python, Go, or Flutter.

### 2. Enable Agent Teams

```bash
# Linux/macOS
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
claude

# Windows PowerShell
$env:CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS = "1"
claude
```

### 3. Set Team Lead Model

```bash
/model opus
```

### 4. Fill Project Setup

Edit
[`project_setup/step_1_project/step_1_project.md`](project_setup/step_1_project/step_1_project.md):

- Project name and project description are the minimum required fields.
- Goals, scope, and other fields may stay `N/A` for agents to propose.
- Release strategy, MVP 1 scope, later-release ideas, and local acceptance
  checks.
- Web/mobile targets.
- Backend, frontend, database, testing, integrations, and deployment intent.
- Runtime secret variable names; actual secret values and concrete
  GitHub/Vercel identifiers stay in `.env`, Vercel, or GitHub Secrets.
- Constraints such as performance, security, compliance, and hard limits.

### 5. Add Optional Inputs

- Requirements: drop Markdown, text, PDF, or image inputs into
  `project_setup/step_2_requirements/`.
- Designs: drop PDF, PNG, JPG, or WebP design exports into
  `project_setup/step_3_design/`.

### 6. Configure MCP and Environment Values

1. Copy `.env.example` to `.env`.
2. Fill only the values needed by your project.
3. Use `[PLACEHOLDER]` when the Team Lead should ask you once.
4. Use `N/A` when the responsible agent should make a reasonable default.
5. Update `.mcp.json` only for MCP servers you intend to use.

Unused MCP servers can be removed from `.mcp.json` before starting Claude Code.

### 7. Kick Off

Paste the prompt from
[`project_kickoff/1_prompt_kickoff.md`](project_kickoff/1_prompt_kickoff.md)
into the Team Lead Claude Code session.

## Project Slash Commands

Claude Code project slash commands live in `.claude/commands/`. Use them when
you want to call one agent directly instead of pasting a full kickoff prompt:

- `/agent-po <task>`
- `/agent-techlead <task>`
- `/agent-designer <task>`
- `/agent-dev <task>`
- `/agent-flutter <task>`
- `/agent-codereview <task>`
- `/agent-qa <task>`
- `/qa-reproduce <bug steps>` for QA-only reproduction of a local acceptance bug.
- `/agent-devops <approval/context>` after explicit deploy or redeploy approval.
- `/release-next <context>` to ask Team Lead for 2-3 next-release options before
  starting another module.

The commands still route through the Team Lead rules, task board, and deployment
gates.

## MCP Servers

| Agent | MCP server | Purpose |
| --- | --- | --- |
| Designer | `figma` | Pull design frames when local design files are absent |
| TechLead/DEV/Flutter | `supabase` | Inspect schema and apply migrations for Supabase projects |
| DevOps | `github` | Push code and configure repository/CI |
| DevOps | `vercel` | Deploy web apps |

MCP configuration lives at root in `.mcp.json`; secrets and concrete identifiers
live in `.env`, copied from `.env.example`.

## Folder Structure

```text
project_setup/
  step_1_project/
    step_1_project.md          # Required project spec
  step_2_requirements/         # Optional PO inputs
  step_3_design/               # Optional design inputs

project_kickoff/
  1_prompt_kickoff.md
  2_prompt_change_request.md

agent_team/
  agents_config.md
  workflow.md
  agents/
  sample_task_board.md
  task_board.md                # Created by Team Lead at kickoff

project_code/
  documentation/
    sample_deliverable/
  backend/                     # Created by DEV Agent when applicable
  frontend/                    # Created by DEV Agent when applicable
  mobile/                      # Created by Flutter Agent when applicable

.claude/
  commands/                    # Project slash commands for individual agents

.env.example
.mcp.json
CLAUDE.md
README.md
```

## Change Requests

After QA or deployment, use
[`project_kickoff/2_prompt_change_request.md`](project_kickoff/2_prompt_change_request.md).
The Team Lead classifies the change and reruns only the required agents.

## Safety Rules

- DevOps never runs without explicit user approval after the interim gate.
- If `deployment.md` exists, redeploy also requires explicit user approval.
- Critical open QA issues block deployment unless the user gives an explicit
  written override.
- Secrets stay in `.env` or GitHub Secrets and must never be committed.
- Claude Code Agent Teams spawn Anthropic models only.

## Examples

Sample outputs are available in
`project_code/documentation/sample_deliverable/`, and a sample board is available
at [`agent_team/sample_task_board.md`](agent_team/sample_task_board.md).

## License

MIT
