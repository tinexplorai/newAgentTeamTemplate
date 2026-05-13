# User Acceptance Bug Reproduction

Created by: QA Agent
Phase: 5 targeted QA-only reproduction
Inputs: user bug report, `interim_report.md`, `qa_report.md`, DEV/Flutter
handoffs, relevant docs

Bug title: Dashboard shows empty activity after login
Report file naming example:
`project_code/documentation/user_acceptance_bug_dashboard-empty-activity.md`

## Environment

- Local URL: `http://localhost:3000`
- Browser/device: Chrome desktop
- Database target: Supabase remote dev project from `SUPABASE_PROJECT_REF`
- Seed data status: seed script rerun before reproduction
- Account/data used: QA seeded account

## User Steps

1. Log in with the seeded QA account.
2. Open the dashboard.
3. Check the recent activity section.

## Expected Result

Dashboard displays seeded recent activity records.

## Actual Result

Dashboard displays the empty state even though seed data exists.

## Evidence

- Screenshot: `project_code/documentation/qa_evidence/dashboard-empty-activity.png`
- Console/network summary: dashboard API returned `200 OK` with empty
  `recentActivity`.

## Reproduction Status

REPRODUCED

## Likely Owner

DEV

Reason: API response shape is valid, but the activity query filters on the wrong
user identifier.

## Recommended Next Action

Team Lead should assign DEV to fix the dashboard activity query, then ask QA to
rerun the targeted dashboard E2E plus affected auth/dashboard regression scope.

QA did not edit production code.
