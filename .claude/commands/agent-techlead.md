---
description: Run the TechLead Agent for API, schema, and technical contract work.
---

Read `CLAUDE.md`, `agent_team/workflow.md`, `agent_team/agents_config.md`, and
`agent_team/agents/Agent_02_TechLead.md`.

Act as Team Lead. Spawn only the TechLead Agent using the model alias from
`agent_team/agents_config.md`.

Task from user:

```text
$ARGUMENTS
```

Preserve prior decisions on change requests. Ensure TechLead reads
`project_code/documentation/source_inventory.md` when present and traces
technical contracts to source IDs. If the task affects API contract, schema,
migration strategy, or architecture, ensure the relevant docs under
`project_code/documentation/` are appended or updated and record the handoff in
`agent_team/task_board.md`.
