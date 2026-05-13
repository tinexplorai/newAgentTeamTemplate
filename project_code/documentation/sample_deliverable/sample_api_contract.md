# API Contract

Created by: TechLead Agent
Phase: 2
Inputs: `project_code/documentation/user_stories.md`,
`project_setup/step_1_project/step_1_project.md`

Selected release scope: MVP 1 - Authenticated Dashboard

## Contract Principles

- Implement only MVP 1 endpoints in this release.
- Keep response shapes stable for future mobile clients.
- Use the same error envelope for all API failures.
- Never expose server-only secrets to frontend or mobile clients.

## Error Format

All error responses use:

```json
{
  "error": {
    "code": "string",
    "message": "string"
  }
}
```

## Authentication

### POST /api/auth/register

Maps to: US-1 AC1-AC5

Registers a new user.

Request:

```json
{
  "email": "user@example.com",
  "password": "string-min-8",
  "confirmPassword": "string-min-8"
}
```

Responses:

- `201 Created`: `{ "userId": "uuid", "email": "user@example.com", "message": "Verification email sent" }`
- `400 Bad Request`: invalid input or password mismatch.
- `409 Conflict`: email already exists.
- `500 Internal Server Error`: unexpected server error.

### POST /api/auth/login

Maps to: US-2 AC1-AC4

Authenticates a verified user.

Request:

```json
{
  "email": "user@example.com",
  "password": "string"
}
```

Responses:

- `200 OK`: `{ "token": "jwt", "user": { "id": "uuid", "email": "user@example.com" } }`
- `400 Bad Request`: invalid input.
- `401 Unauthorized`: invalid credentials.
- `403 Forbidden`: email is not verified.

### POST /api/auth/logout

Maps to: US-2 AC5

Ends the current session.

Responses:

- `204 No Content`: session ended.
- `401 Unauthorized`: missing or invalid token.

### GET /api/auth/verify/{token}

Maps to: US-1 AC5, US-2 AC3

Verifies a registration email token.

Responses:

- `302 Found`: redirects to login success page.
- `400 Bad Request`: invalid or expired token.

## User

### GET /api/user/dashboard

Maps to: US-3 AC1-AC4

Auth: bearer token required.

Responses:

- `200 OK`: `{ "profile": { "id": "uuid", "email": "user@example.com" }, "recentActivity": [] }`
- `401 Unauthorized`: missing or invalid token.
- `404 Not Found`: user not found.

## Environment Boundary

Server-only variables:

- `SUPABASE_SERVICE_ROLE_KEY`
- `JWT_SECRET`
- email-provider API key, if used

Client-safe variables:

- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`

Secret values must not be printed in documentation, logs, task board entries, or
deployment reports.

## Database And Migration Notes

- Database target: Supabase dev project when `SUPABASE_PROJECT_REF` is
  configured.
- DEV must create migration files under `project_code/` and record applied
  migrations in `dev_handoff.md`.
- DEV must keep seed/test data source files and seed/reset commands under
  `project_code/`, preferably `project_code/backend/`, and document where the
  data is stored for QA and user acceptance.
- Destructive migrations, production database changes, or schema changes outside
  this contract require Team Lead escalation.

## Roadmap Compatibility Notes

- MVP 2 profile editing should extend the existing `profiles` table instead of
  introducing a second user profile source.
- Notification preferences should use a separate table keyed by user ID.
- Mobile clients should be able to reuse the same auth and dashboard endpoints.

## Assumptions And N/A Decisions

- JWT session duration is 24 hours.
- Verification tokens expire after 1 hour.
- Backend framework is selected by TechLead based on the generated app stack.

## Handoff

API contract is ready for DEV, Flutter when mobile is in scope, Code Review, and
QA. Any implementation change to method, path, status code, response shape, or
schema requires TechLead review.
