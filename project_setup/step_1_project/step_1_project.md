# Project Description

This is the per-project spec. The agent team reads this file at kickoff for
project-specific context.

Framework-level workflow lives in `../../agent_team/workflow.md`.
Model assignments live in `../../agent_team/agents_config.md`.
Per-agent instructions live in `../../agent_team/agents/`.

Minimum required fields:

- **Name**
- **Project description**

Everything else may stay `N/A`. Agents will decide reasonable defaults using
`agent_team/defaults.md`, document assumptions, and batch any `[PLACEHOLDER]`
asks into a single message when a phase actually needs the value.

## 0. Run Mode

**Autonomy mode:** autonomous

Values:

- `autonomous` (recommended): Team Lead runs Phase 0 through Phase 5 without
  check-ins. Agents document `N/A` decisions using `agent_team/defaults.md`
  instead of asking. Required user touchpoints are only:
  1. Phase 0a Input Echo-back (one summary, reply `go` to proceed).
  2. Phase 5 Interim Gate (run local acceptance, approve deployment).
  3. Any unresolvable `[PLACEHOLDER]` values, batched into one message.
- `ask`: Team Lead may stop at phase boundaries when inputs are ambiguous.

Both modes preserve hard safety gates: deployment requires explicit user
approval, critical QA bugs block deployment, and destructive operations require
confirmation.

## 1. Project Overview

**Name:** [PROJECT NAME]

**Project description:**
[2-4 sentences describing what you are building, who it is for, and why it
matters. Be specific about the user and the problem.]

**Goals / success criteria:**

- N/A

**Out of scope:**

- N/A

## 2. Release Strategy

**Preferred build mode:**

- Phased releases

**Selected first release / MVP 1 scope:**

- N/A

**Known later releases:**

- **MVP 2:** N/A
- **MVP 3:** N/A
- **Later / not now:** N/A

**Priority guidance:**

- N/A

## 3. Users And Core Workflows

**Primary users / roles:**

- N/A

**Core workflows for MVP 1:**

- N/A

**User acceptance checks before deploy:**

- N/A

## 4. Reference Inputs And Fidelity Targets

Use this section when you provide UI PNG/PDF/Figma files or ask the team to
match a website.

**Reference website URLs:**

- N/A

**Reference purpose:**

- N/A

Examples: clone this UI exactly, use as visual inspiration only, match layout
and interactions but use my brand/content, inspect competitor flow for feature
coverage.

**Fidelity target:**

- Exact / Close / Inspired / N/A

**Viewports or devices to match:**

- Desktop: N/A
- Tablet: N/A
- Mobile: N/A

**Design/source priority if inputs conflict:**

- N/A

**Asset usage permission:**

- N/A

Examples: all assets are mine and may be reused; use only assets I provide;
do not copy third-party logos/photos; generate safe replacement imagery.

**Brand/copy exactness:**

- N/A

Examples: use exact copy from PNG; rewrite copy in Vietnamese; preserve brand
names; replace competitor names with my product name.

## 5. Tech Stack

Targets:

- **Web:** yes
- **Mobile:** no

**Backend** (if web = yes):

- **Framework:** N/A
- **Database:** Supabase
- **Other:** N/A

**Web frontend** (if web = yes):

- **Framework:** React or Next.js, TechLead/DEV decides
- **Testing:** Playwright + Vitest/Jest

**Mobile (Flutter)** (if mobile is not no):

- **Min SDK:** N/A
- **State management:** N/A
- **Distribution:** N/A
- **Testing:** flutter test + integration_test

## 6. Data, Integrations, And Runtime Secrets

**Database target for local QA:**

- Supabase remote dev project

**Supabase migration policy** (if Supabase is used):

- Apply migrations to configured Supabase dev project during DEV

**Seed or test data needed:**

- Team Lead/DEV decides per selected release, but QA-passed releases must
  provide deterministic user-acceptance test data, seed/reset commands, and the
  storage target in `project_code/documentation/interim_report.md`.

**Third-party integrations:**

- N/A

**Runtime secret env vars expected:**

- N/A

**Client-safe env vars expected:**

- SUPABASE_URL, SUPABASE_ANON_KEY

## 7. Deployment And Repositories

**Deployment platform intent:**

- GitHub repository + Vercel web deployment after approval

**Deployment identifiers source of truth:**

- Use root `.env` values:
  - `GITHUB_REPO_URL`
  - `GITHUB_DEFAULT_BRANCH`
  - `VERCEL_TEAM_SLUG`
  - `VERCEL_PROJECT_SLUG`

**Smoke test hints:**

- **Web path:** N/A
- **API healthcheck path:** N/A

**Deployment expectation:**

- deploy web after approval

## 8. Constraints And Notes

- **Performance:** N/A
- **Security:** JWT auth, OWASP Top 10 review
- **Scalability:** N/A
- **Compliance:** N/A
- **Hard constraints:** N/A
- **Data sensitivity:** N/A
- **Dependency/license constraints:** N/A
- **Additional notes:** N/A
