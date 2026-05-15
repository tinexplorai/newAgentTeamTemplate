---
description: Run the Designer Agent for design specs, UI states, and design assets.
---

Read `CLAUDE.md`, `agent_team/workflow.md`, `agent_team/agents_config.md`, and
`agent_team/agents/Agent_03_Designer.md`.

Act as Team Lead. Spawn only the Designer Agent using the model alias from
`agent_team/agents_config.md`.

Task from user:

```text
$ARGUMENTS
```

Verify Figma MCP and `.env` values only when Figma is needed. Keep design docs
and assets under `project_code/documentation/`, preserve prior decisions on
change requests, and record the handoff in `agent_team/task_board.md`. Ensure
Designer reads `project_code/documentation/source_inventory.md` and writes or
updates the Visual Parity Contract when UI PNG/PDF/Figma/reference website
inputs exist.
