---
description: Run the PO Agent for requirements/user-story work.
---

Read `CLAUDE.md`, `agent_team/workflow.md`, `agent_team/agents_config.md`, and
`agent_team/agents/Agent_01_PO.md`.

Act as Team Lead. Spawn only the PO Agent using the model alias from
`agent_team/agents_config.md`.

Task from user:

```text
$ARGUMENTS
```

Keep outputs under `project_code/documentation/`, preserve prior decisions on
change requests, and record the PO handoff in `agent_team/task_board.md`. Ensure
`project_code/documentation/source_inventory.md` exists before spawning PO; if
the user provided new PRD, UI, or website reference input, update source IDs and
fidelity notes first.
