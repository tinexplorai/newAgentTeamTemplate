# User Stories

Created by: PO Agent
Phase: 1
Inputs: `project_setup/step_1_project/step_1_project.md`,
`project_setup/step_2_requirements/`

Selected release scope: MVP 1 - Authenticated Dashboard

## Release Plan

| Release | Goal | Included stories | Status |
| --- | --- | --- | --- |
| MVP 1 | Smallest local-testable and deployable authentication flow | US-1, US-2, US-3 | Selected for current build |
| MVP 2 | Profile management and notification preferences | US-4, US-5 | Proposed next |
| Later | Billing, team roles, audit log, analytics | TBD | Deferred |

## MVP 1 Stories

### US-1: User Registration

Priority: P0
Release: MVP 1
Dependencies: Supabase user table, email provider or local email stub.

**As a** new user, **I want to** create an account with email and password, **so
that** I can access the platform.

#### Acceptance Criteria

- [ ] AC1: User can enter email, password, and password confirmation.
- [ ] AC2: System validates email format.
- [ ] AC3: System enforces minimum password strength.
- [ ] AC4: System rejects duplicate email addresses.
- [ ] AC5: System sends or simulates a verification email after successful
  registration.

### US-2: User Login

Priority: P0
Release: MVP 1
Dependencies: US-1, session strategy.

**As a** registered user, **I want to** log in with my credentials, **so that** I
can access my account.

#### Acceptance Criteria

- [ ] AC1: User can enter email and password.
- [ ] AC2: System rejects invalid credentials with a clear error.
- [ ] AC3: System blocks login until email is verified.
- [ ] AC4: System creates a session after successful login.
- [ ] AC5: User can log out.

### US-3: View Dashboard

Priority: P0
Release: MVP 1
Dependencies: US-2, dashboard data query.

**As a** logged-in user, **I want to** see my dashboard, **so that** I can access
the platform's main features.

#### Acceptance Criteria

- [ ] AC1: Dashboard displays profile summary.
- [ ] AC2: Dashboard displays recent activity or an empty state.
- [ ] AC3: Dashboard provides quick links to primary features.
- [ ] AC4: Unauthenticated users are redirected to login.

## Deferred Stories

### US-4: Edit Profile

Priority: P1
Release: MVP 2
Dependencies: US-2, profile table.

Deferred from MVP 1 to keep the first release deployable quickly.

### US-5: Notification Preferences

Priority: P2
Release: MVP 2
Dependencies: US-4, notification provider decision.

Deferred until the notification provider is selected.

## Assumptions And N/A Decisions

- Email verification is required before first login.
- Session expires after 24 hours of inactivity.
- If the email provider is `N/A`, DEV may use a local stub for MVP 1 and
  document the production provider follow-up.

## Release Status

Last updated by: PO Agent after QA handoff

- MVP 1 status: QA PASS WITH NOTES, waiting for user local acceptance.
- Completed stories: US-1, US-2, US-3.
- Failed stories: none.
- Deferred stories: US-4, US-5.
- QA notes: rate limiting and structured auth logging are recommended follow-up
  work but do not block MVP 1 local acceptance.
- PO recommended next MVP/module: MVP 2 - Profile Management.

## Handoff

User stories and release plan are ready for TechLead and Designer. Downstream
agents should implement only the selected release scope unless Team Lead records
an explicit user decision to expand scope.
