# Project Description

This is the per-project spec. The agent team reads this file at kickoff for
project-specific context.

Framework-level workflow lives in `../../agent_team/workflow.md`.
Model assignments live in `../../agent_team/agents_config.md`.
Per-agent instructions live in `../../agent_team/agents/`.

Minimum required fields:

- **Name**
- **Project description**

Everything else may stay `N/A`. Agents will decide reasonable defaults, document
assumptions, and ask once for `[PLACEHOLDER]` values only when needed.

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

## 4. Tech Stack

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

## 5. Data, Integrations, And Runtime Secrets

**Database target for local QA:**

- Supabase remote dev project

**Supabase migration policy** (if Supabase is used):

- Apply migrations to configured Supabase dev project during DEV

**Seed or test data needed:**

- N/A

**Third-party integrations:**

- N/A

**Runtime secret env vars expected:**

- N/A

**Client-safe env vars expected:**

- SUPABASE_URL, SUPABASE_ANON_KEY

## 6. Deployment And Repositories

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

## 7. Constraints And Notes

- **Performance:** N/A
- **Security:** JWT auth, OWASP Top 10 review
- **Scalability:** N/A
- **Compliance:** N/A
- **Hard constraints:** N/A
- **Data sensitivity:** N/A
- **Dependency/license constraints:** N/A
- **Additional notes:** N/A
