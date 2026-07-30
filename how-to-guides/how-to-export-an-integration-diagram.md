# How to Export an Integration Diagram

The toolbar above the [Integration Gateway Workbench](../integration-gateway-platform-reference/integration-gateway-workbench/README.md) canvas lets you export the current integration diagram as an image or as structured data. The toolbar displays the integration name and appears only after you load an integration on the canvas.

## Export Formats

| Action | Output | Use Case |
| --- | --- | --- |
| **Export SVG** | Vector image (`.svg`) | Presentations, Confluence, Figma, Inkscape |
| **Export PNG** | Raster image at 2× resolution (`.png`) | Documents, slide decks |
| **Export JSON** | Integration data as structured JSON (`.json`) | Auditing, version control, external tooling |

All exports capture the full integration graph, not just the visible viewport.

## Export a Diagram

1. Open an integration in the Workbench to display the canvas and toolbar.
2. In the toolbar, click **Export SVG**, **Export PNG**, or **Export JSON**.
3. While the export runs, the button shows an **Exporting...** state and stays disabled. When the export completes, the file downloads automatically.

The system names exported files using the integration name:

* `integration-{name}.svg`
* `integration-{name}.png`
* `integration-{name}.json`

## Export JSON

The **Export JSON** option downloads the integration's data as a structured JSON file. Use it to capture a point-in-time copy of the integration for auditing, version control, or processing in external tooling.
