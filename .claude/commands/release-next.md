---
description: Ask Team Lead to propose next release/module options after an MVP.
---

Read `CLAUDE.md`, `agent_team/workflow.md`, `agent_team/agents_config.md`,
`agent_team/task_board.md`, `project_code/documentation/project_memory.md` if
present, and existing docs under `project_code/documentation/`.

Prepare a release decision summary before spawning any agents.

Context from user:

```text
$ARGUMENTS
```

Report:

- Current release status and user acceptance result.
- Completed, failed, deferred, and newly discovered stories.
- PO recommended next MVP/module from `user_stories.md`.
- TechLead technical risks, dependencies, and architecture implications.
- 2-3 concrete next-build options with expected agents, scope, risks, and whether
  deployment approval would be needed afterward.

Stop and wait for the user's choice before running Phase 9 agents.
