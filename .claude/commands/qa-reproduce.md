---
description: Ask QA only to reproduce a user-reported local acceptance bug.
---

Read `CLAUDE.md`, `agent_team/workflow.md`, `agent_team/agents_config.md`, and
`agent_team/agents/Agent_06_QA.md`.

Act as Team Lead. Spawn only the QA Agent for user acceptance bug triage.

Bug report from user:

```text
$ARGUMENTS
```

Ask QA to read `project_code/documentation/interim_report.md`,
`project_code/documentation/qa_report.md`, DEV/Flutter handoffs, and relevant
docs. QA should run the smallest targeted reproduction needed and write
`project_code/documentation/user_acceptance_bug_<short-title-slug>.md` with
environment, database target, steps, expected result, actual result, evidence,
reproduced/not reproduced status, likely owner, and recommended next action.

QA must not edit production code. Record the QA handoff in
`agent_team/task_board.md`.
