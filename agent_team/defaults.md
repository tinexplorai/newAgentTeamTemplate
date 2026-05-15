# Smart Defaults Catalog

Use this file when a field in `project_setup/step_1_project/step_1_project.md`
or `.env` is `N/A`. The responsible agent picks the default below, documents the
choice in its deliverable, and continues without asking the user.

This file changes only when a default is wrong for the entire template.
Per-project overrides go in `step_1_project.md` (replace the `N/A` with a real
value). Explicit project-spec values always win over this catalog.

## Stack Defaults

| Field | Trigger | N/A default | Reason |
| --- | --- | --- | --- |
| Web framework | `Web: yes`, framework `N/A` | Next.js 14 App Router + TypeScript | Vercel-native; server components and API routes in one app |
| Backend (web only, no separate API hint) | Web `yes`, backend framework `N/A` | Next.js API routes (same app) | Single deploy unit, no separate server |
| Backend (separate API requested) | Web `yes`, backend marked separate | Express + TypeScript | Lowest-friction Node API |
| Database (deploy in scope) | DB `N/A`, deploy intent `yes` | Supabase Postgres | Already template-supported, hosted Postgres + auth |
| Database (local-only or no deploy yet) | DB `N/A`, deploy `no`/`N/A` | SQLite + Drizzle ORM | No external account, fast local QA |
| ORM (Postgres / SQLite) | ORM `N/A` | Drizzle | Type-safe, edge-runtime friendly |
| Auth (web, users mentioned, no Supabase) | Auth `N/A` | JWT + bcrypt + httpOnly cookie | Server-side session, OWASP-safe minimal |
| Auth (Supabase project) | Auth `N/A` and Supabase used | Supabase Auth | Avoid reinventing |
| State management (React) | Frontend state `N/A` | `useState` + Context for shared, TanStack Query for server data | No new dep until needed |
| State management (Flutter) | Mobile state `N/A` | Riverpod | Matches Agent_05_Flutter rules |
| Test runner (web frontend) | Testing `N/A` | Vitest (unit) + Playwright (E2E) | Vite-native + cross-browser |
| Test runner (Node API) | Backend tests `N/A` | Vitest + supertest | Same runner as frontend |
| Test runner (Flutter) | Mobile tests `N/A` | `flutter_test` + `integration_test` | Built-in |
| Email provider (dev/local) | Email `N/A` | Local stub logging to console and `.tmp/emails/*.json` | No external account |
| Email provider (production) | Email `N/A` and deploy `yes` | Document Resend as recommended; do not auto-pick a keyed provider | Requires user-provided key |
| Logger | Logger `N/A` | `console.*` for dev; structured `pino` when a long-running server is mounted | Minimal until needed |
| Styling (web) | CSS `N/A` | Tailwind CSS + shadcn/ui base | Fast match for design specs |
| Form validation | Validation `N/A` | Zod (shared between API and form) | One source of truth |
| Date/time | i18n `N/A` | Native `Intl` + UTC storage | No extra dep until i18n required |
| Mobile distribution channel | `N/A` | TestFlight (iOS) + Play Internal (Android) | First-deploy default |

## Behavior Defaults

| Topic | Default | Notes |
| --- | --- | --- |
| Session expiry | 24h sliding | Documented in API contract |
| Email verification | Required before first login | Unless PRD says otherwise |
| Password rule | min 8 chars, at least 1 letter and 1 digit | Tighten if PRD requires |
| Rate limit | Defer to MVP 2 unless PRD requires | Document as known limitation |
| CORS | Allowlist `http://localhost:3000` plus production URL | DEV documents in handoff |
| Smoke test route (web) | `/` returning 200 | Override via `WEB_SMOKE_TEST_PATH` |
| API healthcheck | `GET /api/health` returning `{ ok: true }` | Override via `API_HEALTHCHECK_PATH` |
| Default theme | Light only | Add dark mode only if PRD/design requires |
| Pagination | Cursor-based when list endpoints exist | Avoid offset for large tables |
| API error envelope | `{ "error": { "code": "...", "message": "..." } }` | Reuse across all endpoints |

## How Agents Use This File

- TechLead reads this file before deciding stack/framework/auth/ORM. If the
  project spec has a real value, use that; otherwise use the row above and
  record the choice in `tech_design.md` `## N/A Decisions`.
- DEV/Flutter read this file before introducing a new dependency category. If
  the row above does not cover the case, document the new choice with a one-line
  reason in `dev_handoff.md` / `flutter_handoff.md`.
- PO does not need this file unless writing assumptions about session/auth
  behavior; in that case mirror the row above so stories match implementation.
- Designer does not need this file (UI defaults live in `design_spec.md`).

## Adding A New Default

1. Try to reuse an existing row before introducing a new one.
2. If most projects would benefit, append a row here with a one-line reason.
3. If the choice is per-project, document it in the agent deliverable instead
   of editing this file.
