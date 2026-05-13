# Design Specification

Created by: Designer Agent
Phase: 2
Inputs: `project_code/documentation/user_stories.md`,
`project_setup/step_3_design/`

Selected release scope: MVP 1 - Authenticated Dashboard

## Design Tokens

- Primary: `#2563EB`
- Success: `#059669`
- Error: `#DC2626`
- Background: `#FFFFFF`
- Surface: `#F9FAFB`
- Text primary: `#111827`
- Text secondary: `#6B7280`
- Font: Inter
- Spacing: `4, 8, 12, 16, 24, 32`
- Radius: `8px`

## Navigation Model

- Public routes: registration, login, email verification result.
- Protected routes: dashboard.
- Unauthenticated users who open a protected route are redirected to login.

## Screen: Registration

Maps to: US-1
Source: inferred from user stories

### Layout

- Centered form region with max width 420px.
- Form spacing 16px.
- Submit button full width.

### Components

- Email input: default, focus, error, disabled.
- Password input: default, focus, error, show/hide toggle.
- Confirm password input: default, focus, error, disabled.
- Submit button: default, hover, loading, disabled.
- Login link.

### Content

- Heading: "Create account"
- CTA: "Sign up"
- Link: "Already have an account? Log in"

### Interactions

- Submit shows loading state.
- Validation errors appear below the related input.
- Success redirects to login with confirmation message.

### Accessibility

- Inputs have visible labels.
- Error messages are associated with inputs.
- Focus order follows visual order.

## Screen: Login

Maps to: US-2
Source: inferred from user stories

### Layout

- Same form pattern as registration.

### Content

- Heading: "Welcome back"
- CTA: "Log in"
- Link: "Forgot password?" only if password reset is in selected release scope.

### Interactions

- Invalid credentials show a non-destructive error message.
- Successful login redirects to dashboard.
- Verified-email success message is shown when redirected from verification.

## Screen: Dashboard

Maps to: US-3
Source: inferred from user stories

### Layout

- Header with product name and user menu.
- Main content area with profile summary and recent activity list.
- Empty activity state when no activity exists.

### Components

- Profile summary.
- Recent activity list.
- Empty activity state.
- Quick links.
- Logout button.

## Responsive Rules

- Mobile: single-column layout, form width fills available space with 16px page
  padding.
- Tablet and desktop: constrained content width and stable form width.
- Avoid text overflow in buttons, cards, and status messages.

## Deferred UI

- Profile editing screens are deferred to MVP 2.
- Notification preferences are deferred to MVP 2.
- Billing, roles, and analytics screens are deferred to Later.

## Assumptions And N/A Decisions

- No local design files were provided.
- Light theme only for MVP 1.
- Use existing project component conventions if the generated app has a design
  system.

## Handoff

Design spec is ready for DEV, Code Review, and QA. If DEV needs to deviate from
this spec for technical reasons, record the deviation in `dev_handoff.md`.
