Start the full SDLC agent-team pipeline for this project.

Read these files first, in order:

1. `project_setup/step_1_project/step_1_project.md` (note `Autonomy mode`)
2. `agent_team/workflow.md`
3. `agent_team/agents_config.md`
4. `agent_team/defaults.md` (smart defaults for `N/A` fields)
5. Enumerate AND read every supported file in
   `project_setup/step_2_requirements/` (PRD, briefs, requirement notes,
   reference website notes, screenshots — `.md`/`.txt`/`.pdf`/`.png`/`.jpg`).
   Do NOT skip; flag unsupported formats like `.docx`/`.pptx`/`.zip` in
   `source_inventory.md` and at Phase 0a echo-back.
6. Enumerate AND read every supported file in `project_setup/step_3_design/`
   (UI mockups, design references — `.pdf`/`.png`/`.jpg`/`.webp`). Same
   no-skip rule.
7. Per-agent prompts in `agent_team/agents/` only when spawning that agent
8. `.env` if it exists; otherwise read `.env.example`. Do not ask for
   `[PLACEHOLDER]` values one by one - batch them at Phase 0a Echo-back

Rules:

- Use the exact model aliases from `agent_team/agents_config.md`.
- Create `agent_team/task_board.md` as the shared project status file.
- Before spawning PO, create
  `project_code/documentation/source_inventory.md` from the project spec, PRD
  files, requirement/design inputs, UI PNG/PDF/Figma references, and any
  reference website URLs. Include source IDs, priority/conflict policy, selected
  release notes, coverage checklist, fidelity targets, asset policy, blockers,
  and assumptions.
- Run input validation as defined in `agent_team/workflow.md` Phase 0:
  enumerate every file in `step_2_requirements/` and `step_3_design/`, list
  unsupported formats under `## Unsupported Inputs` (do NOT silently skip
  `.docx`/`.pptx`/`.zip`), confirm `step_1_project.md` `Name` and
  `Project description` are not template literals, and flag any missing
  referenced files.
- If a website URL is provided, follow the concrete capture procedure in
  `agent_team/workflow.md` Phase 0 (Playwright headless first, then
  `curl --compressed` HTML fallback). Save artifacts under
  `project_code/documentation/source_assets/`. If both fail, record the exact
  blocker; do not silently drop the URL.
- After `source_inventory.md` is written and BEFORE spawning PO, run Phase 0a
  Input Echo-back. Present one summary message containing: project
  understanding (1-2 sentences), inputs detected (counts and IDs), MVP 1
  scope, critical assumptions, smart-default decisions taken from
  `agent_team/defaults.md`, ambiguities worth resolving now, batched
  `[PLACEHOLDER]` asks for the upcoming Phase 1-5 scope, and the autonomy
  plan. Wait for `go` (or a correction) before spawning PO. After confirmation,
  do not interrupt me again until the Phase 5 interim gate.
- Team Lead owns writes to `agent_team/task_board.md`; serialize updates after
  agents finish so parallel agents do not overwrite each other.
- Record each agent's handoff summary in `agent_team/task_board.md`.
- Generated application code must go under `project_code/app/` (web), and
  optionally `project_code/api/` (only when truly split from the web app),
  and/or `project_code/mobile/`.
- Generated documentation must go under `project_code/documentation/`.
- Enforce the missing-value rules from `agent_team/workflow.md`:
  `[PLACEHOLDER]` means batch and ask me once at Phase 0a; `N/A` means the
  responsible agent picks the catalog default from `agent_team/defaults.md`
  and documents the decision in its deliverable.
- Respect `Autonomy mode` from `project_setup/step_1_project/step_1_project.md`.
  Default is `autonomous`: only stop at Phase 0a echo-back, Phase 5 interim
  gate, and unresolvable placeholder batches.
- Ask PO to create a release plan when the project is larger than one small
  release. Record the selected release scope in `agent_team/task_board.md`, and
  keep downstream agents focused on that selected scope unless I explicitly
  expand it.
- Require each downstream deliverable to include source coverage or traceability
  against `source_inventory.md`.
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
- When UI references exist, require Designer to write a Visual Parity Contract,
  DEV/Flutter to provide implementation coverage and screenshot/visual sanity
  evidence when tooling supports it, Code Review to check visual readiness, and
  QA to run a visual parity pass before the interim gate.
- After QA, report QA results back to PO so PO can update `## Release Status` in
  `project_code/documentation/user_stories.md` before the interim report.
- After QA, write `project_code/documentation/interim_report.md` with local
  setup/run/test instructions, source/visual coverage summary, known
  differences from approved sources, and user-acceptance test data. Then stop
  and ask me to run one local acceptance test pass and choose whether to deploy,
  fix something first, or stop here.
- Do not spawn DevOps until I explicitly approve deployment.
- If I approve deployment, run DevOps end to end: push Git, add or update CI
  workflows with smoke tests, deploy through Vercel when web is in scope, write
  `project_code/documentation/deployment.md`, then write
  `project_code/documentation/final_report.md`.
- After MVP 1 or any accepted release, present 2-3 next-build options and wait
  for my choice before starting the next release/module.
