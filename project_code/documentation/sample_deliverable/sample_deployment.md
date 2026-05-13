# Deployment Report

Created by: DevOps Agent
Phase: 6
Date: 2026-05-11

## Preconditions

- QA assessment: PASS WITH NOTES.
- Deployment recommendation: ALLOW.
- User approval: received at Phase 5 gate.
- Critical open issues: none.
- Existing GitHub repository: confirmed.
- Existing Vercel project: confirmed.

## GitHub

- Repository: `https://github.com/username/project-name`
- Default branch: `main`
- Commit pushed: `abc1234`
- CI workflow: `.github/workflows/ci.yml`
- CI status: passing.

## CI Smoke Checks

CI is intentionally limited to smoke/build sanity checks. Full local regression
remains QA-owned before the deploy gate.

Checks added or verified:

- Install backend and frontend dependencies.
- Build frontend.
- Start preview server.
- Smoke `GET /` using `WEB_SMOKE_TEST_PATH`.
- Smoke `GET /api/health` using `API_HEALTHCHECK_PATH`.

Smoke result:

- Route `/`: pass.
- Route `/api/health`: pass.

## Web Deployment

- Platform: Vercel.
- Production URL: `https://project-name.vercel.app`
- Deployment ID: `dpl_example`
- Vercel project slug: `project-name`
- Vercel team slug: `team-name`
- Frontend root: `project_code/frontend`
- Backend/API source: `project_code/backend` when the selected stack supports a
  Vercel-compatible serverless/API adapter.
- Sample backend deployment mode: API exposed through the selected
  Vercel-compatible web stack.
- If the generated backend is a long-running server process, DevOps must record
  the separate hosting requirement instead of marking Vercel deployment complete.

Environment variables configured by name:

- `SUPABASE_URL`
- `SUPABASE_ANON_KEY`
- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- `SUPABASE_SERVICE_ROLE_KEY`
- `JWT_SECRET`
- `NODE_ENV`

Secret values are not printed in this report.

## Production Smoke

- Checked URL: `https://project-name.vercel.app/`
- Checked API route: `https://project-name.vercel.app/api/health`
- Result: pass.

## Failed Deployment Attempts

None.

If a failure occurs, record:

- Deployment ID.
- Failed step.
- Short log summary.
- Error category: DevOps config, application code/build/runtime, account/token,
  missing secret, or smoke-route issue.
- Retry count and next owner.

## Mobile Deployment

Not applicable.

## GitHub Secrets Or Variables Required

Add or verify these in repository settings if CI or deployment uses them:

- `SUPABASE_URL`
- `SUPABASE_ANON_KEY`
- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- `SUPABASE_SERVICE_ROLE_KEY`
- `JWT_SECRET`
- `VERCEL_TOKEN`

Do not paste secret values into documentation.

## First-Deploy Notes

- Test the production registration and login flows.
- Configure custom domain DNS if needed.
- Add monitoring and alerting before public launch.
- Add rate limiting before public marketing traffic.

## Handoff

Deployment completed. Team Lead can compile the final report and prepare
next-release options before starting MVP 2.
