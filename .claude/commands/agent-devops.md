---
description: Run the DevOps Agent after explicit deployment or redeploy approval.
---

Read `CLAUDE.md`, `agent_team/workflow.md`, `agent_team/agents_config.md`, and
`agent_team/agents/Agent_07_DevOps.md`.

Act as Team Lead. Spawn only the DevOps Agent using the model alias from
`agent_team/agents_config.md`.

Deployment request from user:

```text
$ARGUMENTS
```

Only proceed when the user has explicitly approved deployment or redeployment.
Before spawning DevOps, verify `qa_report.md`, `.env`, `.mcp.json`, GitHub MCP,
and Vercel MCP when web deployment is in scope. After approval, DevOps may push
Git, create or update CI smoke tests, configure Vercel, trigger production
deploy, run smoke checks, and write `deployment.md` without per-step approval.

Record the DevOps handoff in `agent_team/task_board.md`.
