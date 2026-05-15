# Step 3 - Design Input

Drop design references here for the Designer Agent. Skip this folder for
backend-only projects.

The Designer Agent resolves design input in this order:

1. Source inventory from Team Lead, including fidelity target and asset policy.
2. Files in this folder.
3. Figma via MCP when `FIGMA_FILE_URL` and `FIGMA_API_KEY` are configured.
4. Reference website URLs recorded in the project spec.
5. Best-effort design spec inferred from user stories.

## Supported Formats

| Format | Status | Notes |
| --- | --- | --- |
| PDF | Preferred | Best for multi-screen exports |
| PNG/JPG/WebP | Supported | Use one image per screen/frame |
| PPT/PPTX | Convert first | Export to PDF |
| ZIP | Unzip first | Drop extracted files here |

When providing UI screenshots, use one file per screen/state when possible.
Name stateful variants clearly, for example `02_checkout_error_mobile.png`.
Team Lead will list these files in
`project_code/documentation/source_inventory.md`, and Designer will convert
them into a Visual Parity Contract for implementation and QA.

## Naming

Use number prefixes when order matters:

```text
project_setup/step_3_design/
  01_login.png
  02_dashboard.png
  03_settings.pdf
  style_guide.pdf
```
