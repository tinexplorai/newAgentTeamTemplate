CHANGE REQUEST - rerun a scoped subset of the agent team after QA or deployment.

Process:

1. Ground yourself. Read in this order:
   - `agent_team/workflow.md`, especially Phase 9.
   - `agent_team/agents_config.md`.
   - Relevant prompts in `agent_team/agents/` for agents that may run.
   - `project_setup/step_1_project/step_1_project.md`.
   - `agent_team/task_board.md`.
   - Existing docs in `project_code/documentation/`:
     `source_inventory.md`, `user_stories.md`, `design_spec.md`,
     `api_contract.md`, `qa_report.md`, and `deployment.md` if it exists.
   - New PRD, UI PNG/PDF/Figma, or reference website inputs mentioned in this
     request.

2. Classify the change as one of:
   - QA-only triage.
   - Small.
   - Medium.
   - Large-backend.
   - Large-UX.

3. Report back before spawning agents:
   - Classification and reasoning.
   - Agents to run and order.
   - Target release/module and whether it changes the selected release scope.
   - For a new release after MVP 1 or an accepted release, 2-3 next-build
     options and a recommendation before selecting agents.
   - Docs that will receive appended sections.
   - Whether `source_inventory.md` needs new source IDs, changed fidelity
     targets, or updated asset policy.
   - Likely generated-code areas under `project_code/`.
   - Whether Code Review will run before QA.
   - Whether visual parity checks are required.
   - Ambiguities that would change the plan.
   - A filesystem-safe short title slug for reports.

4. Stop and wait for my approval or release/module selection before spawning
   agents.

5. After approval:
   - Open a new section in `agent_team/task_board.md`:
      `## Phase 9 - Change Request: <short title>`.
   - Spawn only the agents required by the classification.
   - Tell every agent to append to existing docs instead of rewriting prior
     sections.
   - Update `project_code/documentation/source_inventory.md` first when the
     change includes new PRD files, screenshots, Figma frames, or reference
     website URLs. Assign new source IDs and preserve prior source history.
   - Keep the change scoped to the target release/module unless I explicitly
     expand scope.
   - Run Code Review before QA whenever application code changed. Critical
     code-review findings block QA until the assigned owner fixes them.
   - Keep edits scoped to the classified change unless a wider dependency is
     explicitly needed and documented.
   - Keep all application changes under `project_code/`.
   - Never print secret values into the task board, reports, or handoff docs.
   - For QA-only triage, spawn QA Agent only and ask it to write
     `project_code/documentation/user_acceptance_bug_<short-title-slug>.md`.

6. QA must run regression for every flow sharing code with the changed area, not
   only the new behavior. When the change touches UI reference sources, QA must
   also rerun the affected visual parity checks from `design_spec.md`. If
   regression fails, QA records evidence, severity, and likely owner; Team Lead
   assigns the fix to the appropriate agent or user config owner. After the
   owner fixes the issue, QA reruns the failed tests plus affected regression
   scope before reporting PASS. If it cannot be fixed within the scoped change,
   write the current QA status, blockers, and recommended next agent handoff in
   the task board and change report.

7. After QA passes, write
   `project_code/documentation/change_report_<short-title-slug>.md` with:
   - What changed.
   - Which agents ran.
   - QA results and regression deltas versus the previous `qa_report.md`.
   - Source coverage and visual parity deltas when applicable.
   - Files touched, paths only.
   - Release plan/status update made by PO, when PO ran.

8. If `project_code/documentation/deployment.md` exists, stop and ask:
   "Please run one local acceptance test pass. Redeploy via DevOps Agent? Or fix
   something first?" Do not spawn DevOps without my explicit approval. If I
   approve redeploy, run DevOps end to end: push Git, add or update CI smoke
   tests, deploy through Vercel when web is in scope, update deployment
   documentation with the deployment delta, then update or write
   `project_code/documentation/final_report.md`.

My change request:
=== START ===
What:        [describe the change in plain language]
Where:       [screen / endpoint / file area, leave blank if unknown]
Why:         [user feedback / new requirement / bug context]
Severity:    [blocker / important / nice-to-have]
Constraints: [compatibility, deadline, areas not to touch, etc.]
=== END ===
