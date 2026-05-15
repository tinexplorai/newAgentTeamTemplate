# Step 2 - Requirements Input

Drop reference material here for the PO Agent.

The PO Agent reads:

1. `project_setup/step_1_project/step_1_project.md`.
2. Every supported file in this folder.

If this folder is empty, PO writes user stories from the project spec and notes
assumptions.

## Supported Formats

| Format | Status | Notes |
| --- | --- | --- |
| Markdown (`.md`) | Preferred | Easiest to inspect and cite |
| Plain text (`.txt`) | Supported | Good for briefs and notes |
| PDF | Supported | Prefer concise PDFs |
| PNG/JPG screenshots | Supported | Useful for sketches or support-ticket captures |
| Website/reference notes (`.md`/`.txt`) | Supported | Include URL, pages/routes, login steps, fidelity target, and what to ignore |
| DOC/DOCX | Convert first | Convert to Markdown or PDF |
| PPT/PPTX | Convert first | Export as PDF |

## Naming

Use number prefixes when order matters:

```text
project_setup/step_2_requirements/
  01_product_brief.md
  02_customer_interviews.md
  03_competitor_notes.pdf
  04_reference_site_notes.md
```

Put canonical reference website URLs and fidelity targets in
`project_setup/step_1_project/step_1_project.md` so Team Lead can include them
in `project_code/documentation/source_inventory.md` before PO runs.
