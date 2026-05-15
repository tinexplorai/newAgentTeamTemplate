---
description: Run the Code Review Agent for implementation readiness review.
---

Read `CLAUDE.md`, `agent_team/workflow.md`, `agent_team/agents_config.md`, and
`agent_team/agents/Agent_08_CodeReview.md`.

Act as Team Lead. Spawn only the Code Review Agent using the model alias from
`agent_team/agents_config.md`.

Task from user:

```text
$ARGUMENTS
```

Code Review must not edit production code. It reviews implementation quality,
contract adherence, security/env boundaries, database safety, handoff readiness,
source coverage, visual parity readiness when UI references exist, and QA
readiness. It writes `project_code/documentation/code_review.md` and returns
findings with likely owners. Record the handoff in `agent_team/task_board.md`.
