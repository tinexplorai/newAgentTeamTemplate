# Step 1 - Project Spec

Edit `step_1_project.md` before kickoff. This file is the per-project source of
truth for the agent team.

## What To Fill

- Project name and project description. These are the minimum required fields.
- Goals and success criteria.
- Out-of-scope items.
- Release strategy, MVP 1 scope, later-release ideas, and priority guidance.
- Primary user roles, MVP 1 workflows, and local acceptance checks.
- Reference website URLs, UI fidelity target (`Exact`, `Close`, or `Inspired`),
  target viewports/devices, source priority, asset-use permission, and
  brand/copy exactness for UI PNG/Figma/website matching.
- Web and/or mobile targets.
- Backend, frontend, database, testing, and distribution choices.
- Database target for local QA, migration policy, seed/test data, integrations,
  and runtime secret env var names.
- Deployment platform intent and smoke-test hints. Concrete GitHub/Vercel
  identifiers belong in root `.env`.
- Constraints such as security, performance, compliance, scalability, and hard
  platform limits.

## Missing-Value Markers

| Marker | Meaning |
| --- | --- |
| Real value | Agents use it as-is |
| `[PLACEHOLDER]` | Team Lead asks you once and writes the answer back |
| `N/A` | Responsible agent chooses a reasonable default and documents it |

## Related Inputs

- Requirements: `../step_2_requirements/`.
- Design references: `../step_3_design/`.
- Source inventory generated at kickoff:
  `../../project_code/documentation/source_inventory.md`.
- Secrets and external identifiers: root `.env`.
- Workflow rules: `../../agent_team/workflow.md`.
