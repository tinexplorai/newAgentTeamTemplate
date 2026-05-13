Start the full SDLC agent-team pipeline for this project.

Read these files first, in order:

1. `project_setup/step_1_project/step_1_project.md`
2. `agent_team/workflow.md`
3. `agent_team/agents_config.md`
4. Every required prompt in `agent_team/agents/`
5. `.env` if it exists; otherwise read `.env.example` and ask me to create
   `.env` only when the current phase needs a missing value

Rules:

- Use the exact model aliases from `agent_team/agents_config.md`.
- Create `agent_team/task_board.md` as the shared project status file.
- Team Lead owns writes to `agent_team/task_board.md`; serialize updates after
  agents finish so parallel agents do not overwrite each other.
- Record each agent's handoff summary in `agent_team/task_board.md`.
- Generated application code must go under `project_code/backend/`,
  `project_code/frontend/`, and/or `project_code/mobile/`.
- Generated documentation must go under `project_code/documentation/`.
- Enforce the missing-value rules from `agent_team/workflow.md`:
  `[PLACEHOLDER]` means ask me once; `N/A` means the responsible agent decides
  and documents the decision.
- Ask PO to create a release plan when the project is larger than one small
  release. Record the selected release scope in `agent_team/task_board.md`, and
  keep downstream agents focused on that selected scope unless I explicitly
  expand it.
- Ask TechLead to keep a product-wide architecture view across planned MVPs,
  while writing detailed implementation contracts only for the selected release.
- When I provide a value for `[PLACEHOLDER]`, write it back to the appropriate
  source file (`.env` for environment values or
  `project_setup/step_1_project/step_1_project.md` for project-spec values).
- Never print secret values into the task board, reports, or handoff docs. Use
  env var names only.
- Before spawning Designer or DevOps, verify the MCP server and `.env` values
  they need are configured. Ask once for required missing values.
- Follow the process in `agent_team/workflow.md` through QA.
- When application code changed, run Code Review after DEV/Flutter and before
  QA. Critical code-review findings block QA until the assigned owner fixes
  them.
- After QA, report QA results back to PO so PO can update `## Release Status` in
  `project_code/documentation/user_stories.md` before the interim report.
- After QA, write `project_code/documentation/interim_report.md` with local
  setup/run/test instructions, then stop and ask me to run one local acceptance
  test pass and choose whether to deploy, fix something first, or stop here.
- Do not spawn DevOps until I explicitly approve deployment.
- If I approve deployment, run DevOps end to end: push Git, add or update CI
  workflows with smoke tests, deploy through Vercel when web is in scope, write
  `project_code/documentation/deployment.md`, then write
  `project_code/documentation/final_report.md`.
- After MVP 1 or any accepted release, present 2-3 next-build options and wait
  for my choice before starting the next release/module.
