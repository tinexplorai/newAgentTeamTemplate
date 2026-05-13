# Step 3 - Design Input

Drop design references here for the Designer Agent. Skip this folder for
backend-only projects.

The Designer Agent resolves design input in this order:

1. Files in this folder.
2. Figma via MCP when `FIGMA_FILE_URL` and `FIGMA_API_KEY` are configured.
3. Best-effort design spec inferred from user stories.

## Supported Formats

| Format | Status | Notes |
| --- | --- | --- |
| PDF | Preferred | Best for multi-screen exports |
| PNG/JPG/WebP | Supported | Use one image per screen/frame |
| PPT/PPTX | Convert first | Export to PDF |
| ZIP | Unzip first | Drop extracted files here |

## Naming

Use number prefixes when order matters:

```text
project_setup/step_3_design/
  01_login.png
  02_dashboard.png
  03_settings.pdf
  style_guide.pdf
```
