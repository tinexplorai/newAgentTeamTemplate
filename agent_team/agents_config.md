# Agent Configuration

This is the single place to change which model each agent uses. The Team Lead
must read this file before spawning agents and pass the exact model alias shown
below.

## Models

| Agent | Model | Description file | MCP servers used |
| --- | --- | --- | --- |
| Team Lead | `opus` | Main Claude Code session; owns source intake and gates | As needed |
| PO Agent | `opus` | `agents/Agent_01_PO.md` | None |
| TechLead Agent | `opus` | `agents/Agent_02_TechLead.md` | `supabase` when DB is Supabase |
| Designer Agent | `opus` | `agents/Agent_03_Designer.md` | `figma` when no local design files exist |
| DEV Agent | `sonnet` | `agents/Agent_04_DEV.md` | `supabase` when DB is Supabase |
| Flutter Agent | `sonnet` | `agents/Agent_05_Flutter.md` | `supabase` when DB is Supabase |
| Code Review Agent | `sonnet` | `agents/Agent_08_CodeReview.md` | None |
| QA Agent | `sonnet` | `agents/Agent_06_QA.md` | None |
| DevOps Agent | `sonnet` | `agents/Agent_07_DevOps.md` | `github`, `vercel` for web deploys |

## Available Model Aliases

| Alias | Use |
| --- | --- |
| `opus` | Deep reasoning: Team Lead, PO, TechLead, Designer |
| `sonnet` | Balanced execution: DEV, Flutter, Code Review, QA, DevOps |
| `haiku` | Fast low-cost summaries or lookups |

Aliases resolve to the latest Claude Code model version. Pin a specific version
only if reproducibility matters more than automatic upgrades.

## Execution Policy

This file only defines model and MCP assignments. Agent autonomy, local command
execution, phase gates, and reporting rules live in `agent_team/workflow.md`.

DEV and Flutter Agents are expected to work autonomously within approved project
scope until their implementation handoff is complete. Do not downgrade them to
lookup-only or ask-before-every-edit behavior unless the user explicitly pauses
the implementation.

## Provider Note

Claude Code Agent Teams spawn Anthropic models through the Agent tool. This
template does not support ChatGPT, Gemini, or other providers without a
different orchestrator.

## MCP Servers

MCP configuration lives in root `.mcp.json`. Secrets and concrete identifiers
live in root `.env`, copied from root `.env.example`.

If a project does not need an MCP server, remove that server block from
`.mcp.json` before restarting Claude Code.

## Adding A New Agent

1. Add a row to the Models table.
2. Create an instruction file in `agent_team/agents/`.
3. Reference the agent in `agent_team/workflow.md`.
4. Add any required MCP server to root `.mcp.json`.
5. Add any required environment values to root `.env.example`.
