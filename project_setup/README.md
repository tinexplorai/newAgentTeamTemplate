# Project Setup

These files are the user-editable inputs for a new project. Fill them before
running the kickoff prompt.

## Step 1 - Project Spec

Edit `step_1_project/step_1_project.md`.

Minimum required fields are project name and project description. Most other
fields default to `N/A` or reasonable web/Supabase/Vercel defaults so PO,
TechLead, and DEV can propose the plan.

Use:

- Real values when known.
- `[PLACEHOLDER]` when the Team Lead should ask you once.
- `N/A` when you want the responsible agent to decide and document the choice.

For broad products, fill the release strategy and MVP 1 scope carefully. The
agent team will build only the selected release scope first, then Team Lead will
ask before starting MVP 2 or later modules.

If you want the team to match a UI PNG, Figma frame, or website, fill
`Reference Inputs And Fidelity Targets` in the project spec. Use `Exact` for
near pixel/copy parity, `Close` for same structure and feel, and `Inspired` when
the source is only creative direction. Also state whether third-party logos,
photos, and brand assets may be reused.

## Step 2 - Requirements

Drop optional PO inputs into `step_2_requirements/`:

- Markdown.
- Plain text.
- PDF.
- PNG/JPG screenshots.
- Website/reference notes, route lists, login steps, or competitor-flow notes in
  Markdown/text.

DOC/DOCX and PPT/PPTX should be converted to Markdown or PDF first.

## Step 3 - Design Inputs

Drop optional design references into `step_3_design/`:

- PDF.
- PNG/JPG/WebP.

If this folder is empty and Figma is configured in `.env` plus `.mcp.json`, the
Designer Agent can use Figma. If neither local designs nor Figma are available,
Designer writes a best-effort spec from user stories.

Reference website URLs should go in the Step 1 project spec so Team Lead can
capture them in `project_code/documentation/source_inventory.md` before PO and
Designer run.

## Secrets And External IDs

Copy root `.env.example` to root `.env` and fill the values needed by the
project. Keep secrets out of Git.

Put only variable names in the project spec. Secret values belong in `.env`,
Vercel env vars, or GitHub Secrets.

MCP server configuration lives in root `.mcp.json`.

## Kickoff

After setup, paste `project_kickoff/1_prompt_kickoff.md` into Claude Code.
