# Project-Level Claude Code Configuration

This folder holds project-scoped Claude Code config:

- `commands/` - project slash commands like `/agent-dev`, `/agent-qa` (committed).
- `settings.local.example.json` - **template** for project-specific permissions
  (committed as example).
- `settings.local.json` - **your actual project permissions**, gitignored.

## Two-tier permission model

Permissions live in two places. Use both correctly to avoid the "old project's
allowlist polluting the new project" problem.

| Tier | File | Scope | Example entries |
| --- | --- | --- | --- |
| User-level | `~/.claude/settings.json` (Windows: `C:\Users\<you>\.claude\settings.json`) | Every project on this machine | `Bash(npm run *)`, `Bash(npx playwright *)`, `mcp__supabase__list_tables`, `gh secret set *` |
| Project-level | `<this-project>/.claude/settings.local.json` | This project only | `Bash(curl -sI https://my-app.vercel.app/*)`, `Bash(git -C my-app push origin main)`, `Bash(PLAYWRIGHT_BASE_URL=https://my-app.vercel.app npx playwright test *)` |

Generic patterns (commands that look the same across every project) belong at
user level. Project-specific patterns (commands that contain THIS project's
URL, repo name, or absolute path) belong at project level.

## Setup for a new project

1. Copy the template:

   ```powershell
   Copy-Item .claude\settings.local.example.json .claude\settings.local.json
   ```

2. Open `.claude/settings.local.json` and replace every `<PLACEHOLDER>`:
   - `<PROJECT_NAME>` -> kebab-case project name (e.g. `my-app`)
   - `<PROJECT_DIR>` -> absolute path to your generated app root
   - `<VERCEL_URL>` -> deployed web hostname (after first deploy; leave the
     entries in place and they activate when the URL exists)
   - `<API_URL>` -> deployed API hostname when API is split (skip if not
     applicable)

3. Restart Claude Code so it picks up the new file.

4. Delete entries you don't need (mobile-only project doesn't need
   `cd project_code/app` patterns, etc.).

## When Claude Code asks "Always allow?"

Claude Code's "Always allow" by default writes to user-level settings. For
project-specific commands, the cleaner choice is to add them manually to
`settings.local.json` instead of clicking "Always allow" - that way they stay
scoped to this project and disappear when the folder is deleted.

If a command looks generic (`npm run lint`), letting it go to user-level is
fine. If it contains a URL/path/credential, prefer project-level.

## Why two tiers

- **User-level** stays small and reusable. New projects inherit a sensible
  default permission set on day one.
- **Project-level** is gitignored so credentials and absolute paths never
  leak into Git history or to teammates.
- When you archive the project folder, its specific permissions die with it -
  no orphaned entries cluttering `~/.claude/settings.json` 6 months later.

## Periodic cleanup

Every 1-2 months, ask Team Lead to scan `~/.claude/settings.json` for entries
hardcoded to projects you no longer work on, and clean them out. The
`/fewer-permission-prompts` skill can also propose project-level additions
based on recent transcripts.
