# SDLC Agent Team Framework

> A reusable framework for building software with **Claude Code Agent Teams**.  
> Each AI agent plays a specialized role (PO, TechLead, Designer, DEV, QA, DevOps) and coordinates through shared files — just like a real dev team.

---

## Overview

This template orchestrates multiple Claude agents to build complete software projects:
- **Team Lead** (you) coordinates the workflow
- **Specialist agents** handle requirements, design, implementation, testing, and deployment
- **Task board** tracks progress and agent communication
- **Deliverables** are written to `project_code/documentation/`

**Works for:** SaaS, APIs, mobile apps, dashboards, e-commerce, chat apps, etc.

---

## Agent Team

| Agent | Model | Role |
|-------|-------|------|
| **Team Lead** | opus | Orchestrates workflow, spawns agents, writes reports |
| **PO Agent** | opus | Analyzes requirements → User Stories |
| **TechLead Agent** | opus | Designs API contract + technical architecture |
| **Designer Agent** | sonnet | Translates designs → UI specification |
| **DEV Agent** | sonnet | Implements backend + frontend + tests |
| **Flutter Agent** | sonnet | Builds mobile app (Android/iOS) |
| **QA Agent** | sonnet | Runs tests, reviews code, reports issues |
| **DevOps Agent** | sonnet | Pushes to GitHub, sets up CI/CD, deploys |

**Model configuration:** Edit [agent_team/agents_config.md](agent_team/agents_config.md)

---

## Workflow

### Phase 1: Requirements
**PO Agent** reads `project_setup/` and writes `project_code/documentation/user_stories.md`

### Phase 2: Design & Architecture (Parallel)
- **TechLead Agent** → `api_contract.md` + `tech_design.md`
- **Designer Agent** → `design_spec.md`

### Phase 3: Implementation
- **Web:** DEV Agent → backend + frontend
- **Mobile:** Flutter Agent → iOS/Android app
- **Both:** DEV first, then Flutter (sequential)

### Phase 4: Quality Assurance
**QA Agent** runs all tests locally → `qa_report.md`

### Phase 5: Interim Report
**Team Lead** compiles results → `interim_report.md`  
**🛑 HUMAN GATE:** User approves deployment

### Phase 6: Deployment
**DevOps Agent** pushes to GitHub, sets up CI/CD, deploys → `deployment.md`

### Phase 7: Final Report
**Team Lead** → `final_report.md` with deployment URLs and next steps

**See:** [agent_team/workflow.md](agent_team/workflow.md) for full details

---

## Quick Start

### 1. Prerequisites
- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) or VS Code Extension
- Git
- Runtime for your stack (Node.js, Python, Go, etc.)

### 2. Enable Agent Teams
```bash
# Linux/macOS
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
claude

# Windows PowerShell
$env:CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS = "1"
claude
```

### 3. Set Model
```bash
/model opus    # Team Lead should use most capable model
```

### 4. Fill Project Setup
Edit `project_setup/step_1_project/step_1_project.md`:
- Project name, description, goals
- Tech stack (web/mobile, backend, frontend, database)
- Constraints

### 5. Add Requirements (Optional)
Drop files into `project_setup/step_2_requirements/`:
- PRDs, briefs, customer interviews, screenshots, etc.

### 6. Add Designs (Optional)
Drop files into `project_setup/step_3_design/`:
- PDFs, PNGs, JPGs from Figma/Sketch/etc.

### 7. Kickoff
Paste the prompt from [project_kickoff/1_prompt_kickoff.md](project_kickoff/1_prompt_kickoff.md)

---

## Configuration

### MCP Servers (Optional)
Some agents use external tools via [MCP](https://modelcontextprotocol.io):

| Agent | MCP Server | Purpose |
|-------|------------|---------|
| Designer | `figma` | Pull design frames (if no local files) |
| TechLead/DEV/Flutter | `supabase` | Database schema + migrations |
| DevOps | `github` | Push code, CI/CD setup |
| DevOps | `vercel` | Deploy web apps |

**Setup:**
1. Copy `project_code/.env.example` → `project_code/.env`
2. Add API tokens (Figma, GitHub, Vercel, Supabase)
3. Update `project_code/.mcp.json` with your project slugs
4. Restart Claude Code

**Skip what you don't need** — agents adapt to available tools.

---

## Folder Structure

```
├── project_setup/           # Per-project inputs
│   ├── step_1_project/      # Project spec (name, stack, goals)
│   ├── step_2_requirements/ # PRDs, briefs, research
│   └── step_3_design/       # Design files (PDF, PNG, JPG)
│
├── project_kickoff/         # Prompt templates
│   ├── 1_prompt_kickoff.md
│   └── 2_prompt_change_request.md
│
├── agent_team/              # Framework configuration
│   ├── agents_config.md     # Model assignments
│   ├── workflow.md          # Process details
│   ├── agents/              # Per-agent instructions
│   └── sample_task_board.md # Example task board
│
└── project_code/            # Generated output
    ├── .env.example         # Environment template
    ├── .mcp.json            # MCP configuration
    ├── documentation/       # Agent deliverables
    │   └── sample_deliverable/  # Examples
    ├── backend/             # Created by DEV Agent
    ├── frontend/            # Created by DEV Agent
    └── mobile/              # Created by Flutter Agent
```

---

## Change Requests

After initial deployment, use [project_kickoff/2_prompt_change_request.md](project_kickoff/2_prompt_change_request.md) to:
- Fix bugs
- Add features
- Update designs
- Modify APIs

Team Lead re-runs only the necessary agents (not the full pipeline).

---

## Examples

See `project_code/documentation/sample_deliverable/` for example outputs:
- `sample_user_stories.md`
- `sample_api_contract.md`
- `sample_design_spec.md`
- `sample_qa_report.md`
- `sample_deployment.md`
- `sample_interim_report.md`
- `sample_final_report.md`

See `agent_team/sample_task_board.md` for workflow example.

---

## Notes

- **Provider:** Uses Claude Code's native `Agent` tool (Anthropic models only)
- **Communication:** Agents coordinate via `task_board.md`, not chat
- **Deployment gate:** User approval required before DevOps runs
- **Customization:** Edit agent instructions in `agent_team/agents/`

---

## License

MIT
