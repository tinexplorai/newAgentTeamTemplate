# Technical Design

Created by: TechLead Agent
Phase: 2
Inputs: `project_code/documentation/user_stories.md`,
`project_code/documentation/api_contract.md`,
`project_setup/step_1_project/step_1_project.md`

Selected release scope: MVP 1 - Authenticated Dashboard

## Product-Wide Architecture View

MVP 1 should not block later releases for profile editing, notification
preferences, mobile clients, or team roles. The data model keeps user identity,
profile data, activity events, and future preferences separate.

## Selected Stack

- Frontend: Next.js or React, final choice by DEV based on project setup.
- Backend: TechLead-selected API layer compatible with the frontend choice.
- Database: Supabase.
- Deployment target: Vercel for web.
- Test framework: Vitest/Jest for unit tests and Playwright for web E2E.

## Architecture

- Web UI reads and writes only through the documented API layer.
- API layer owns validation, auth checks, and server-only Supabase operations.
- Supabase stores user profile and dashboard activity data.
- Email verification can use a local stub in MVP 1 if the production provider is
  not configured yet.

## Data Model

### profiles

- `id`: UUID primary key, linked to auth user ID.
- `email`: text, unique, required.
- `display_name`: text, nullable for MVP 1.
- `created_at`: timestamp.
- `updated_at`: timestamp.

### activity_events

- `id`: UUID primary key.
- `user_id`: UUID, references `profiles.id`.
- `event_type`: text.
- `metadata`: JSON.
- `created_at`: timestamp.

## Migration Strategy

- DEV creates migration files under `project_code/`.
- If Supabase MCP values are configured, migrations may be applied to the
  configured Supabase dev project.
- DEV records migration names, commands/tools used, seed behavior, and results
  in `dev_handoff.md`.
- Destructive migrations or production database changes require Team Lead
  escalation.

## Environment Boundary

Server-only:

- `SUPABASE_SERVICE_ROLE_KEY`
- `JWT_SECRET`
- email-provider secret key, if used

Client-safe:

- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`

## Local Testing Target

- Web frontend: `http://localhost:3000`.
- API: framework-dependent local URL, documented by DEV.
- E2E database: Supabase dev project unless DEV documents a local database or
  mock target.

## Roadmap Compatibility

- MVP 2 profile editing extends `profiles`.
- MVP 2 notification preferences should use a separate table keyed by user ID.
- Later team roles should introduce organization/team tables without rewriting
  MVP 1 profile identity.

## Risks And Mitigations

- Missing Supabase token/ref: DEV writes migration files locally and records the
  blocker.
- Missing email provider: use local verification stub for MVP 1 and document the
  production gap.
- Auth security: add rate limiting and structured logging before public launch.

## Handoff

TechLead contract and technical design are ready for DEV, Code Review, QA, and
DevOps. Any schema, auth, or API shape change should return to TechLead before
implementation continues.
