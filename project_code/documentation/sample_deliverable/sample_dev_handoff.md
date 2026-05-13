# DEV Handoff

Created by: DEV Agent
Phase: 3
Inputs: TechLead docs, Designer docs, selected release scope

## Scope Completed

- Built MVP 1 web/backend implementation.
- Created backend API endpoints from `api_contract.md`.
- Created frontend screens from `design_spec.md`.
- Added local tests and Playwright E2E coverage for primary flows.
- Added Supabase migration and seed scripts.

## Generated Paths

- Backend: `project_code/backend/`
- Frontend: `project_code/frontend/`
- Migrations: `project_code/backend/supabase/migrations/`
- E2E tests: `project_code/frontend/tests/e2e/`

## Install Commands

```bash
cd project_code/backend && npm install
cd project_code/frontend && npm install
```

## Run Commands

```bash
cd project_code/backend && npm run dev
cd project_code/frontend && npm run dev
```

Expected local URLs:

- Frontend: `http://localhost:3000`
- API: `http://localhost:4000`

## Test Commands

```bash
cd project_code/backend && npm test
cd project_code/frontend && npm test
cd project_code/frontend && npx playwright test
```

## Build Commands

```bash
cd project_code/frontend && npm run build
```

## Required Env Var Names

- `SUPABASE_URL`
- `SUPABASE_ANON_KEY`
- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- `SUPABASE_SERVICE_ROLE_KEY`
- `SUPABASE_PROJECT_REF`
- `JWT_SECRET`

Secret values are not printed in this handoff.

## Database And Migrations

- Target: Supabase remote dev project from `SUPABASE_PROJECT_REF`.
- Migration created: `202605110001_auth_profiles.sql`.
- Migration applied: yes.
- Seed command: `npm run db:seed`.
- Seed data: QA test user and dashboard activity.
- Production database touched: no.

## Verification Results

- Backend tests: pass.
- Frontend unit tests: pass.
- Frontend build: pass.
- Playwright smoke path: pass locally.

## Known Limitations

- Auth rate limiting is not implemented in MVP 1.
- Structured failed-login logging is not implemented in MVP 1.
- Production email provider values must be configured before public traffic.

## Handoff

Implementation is ready for Code Review. If Code Review or QA finds issues, Team
Lead should assign the fix back to DEV for application code, migration, seed, or
local-run defects.
