---
description: Run the QA Agent for verification, triage, or reruns after fixes.
---

Read `CLAUDE.md`, `agent_team/workflow.md`, `agent_team/agents_config.md`, and
`agent_team/agents/Agent_06_QA.md`.

Act as Team Lead. Spawn only the QA Agent using the model alias from
`agent_team/agents_config.md`.

Task from user:

```text
$ARGUMENTS
```

QA must not edit production code. QA owns verification, visual parity checks
when UI references exist, source coverage validation, triage, evidence, likely
owner recommendation, and reruns after the assigned owner fixes issues. Record
the QA handoff and report paths in `agent_team/task_board.md`.
