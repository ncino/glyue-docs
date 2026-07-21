---
description: Visualize an integration as an interactive flow diagram
---

# Integration Gateway Workbench

The Integration Gateway Workbench is a visual interface that displays an integration's components as an interactive flow diagram. You can inspect an integration's structure — its service requests, field mappings, value mapping sets, and validation rules — directly on the canvas instead of on the Build page.

In the 4.21 release, the Workbench is read-only: it visualizes existing integrations but does not edit them. Each node provides an **Open in Build** link to make changes.

{% hint style="info" %}
The Workbench is available behind a feature flag. When you enable the flag, a **Workbench** entry appears in the sidebar navigation.
{% endhint %}

## Access the Workbench

To open an integration in the Workbench:

1. Click **Workbench** in the sidebar navigation. The welcome overlay displays a **Select an Integration** heading.
2. From the **Integration** drop-down menu, select an integration to visualize.
3. Click **Open**.

The canvas loads at `/workbench/:integrationId` and displays the integration's components as a connected node diagram.

## Navigate the Canvas

The canvas supports these navigation controls:

* **Zoom** — Scroll to zoom in and out. The node connection dots grow as you zoom in.
* **Pan** — Click and drag the canvas background to move around.
* **Minimap** — The minimap in the corner provides an overview of the full diagram and your current position within it.

Edges connect the nodes and represent the integration's execution flow. The canvas displays four node types: Integration, Service Request, Field Mapping, and Validation Rule. For a description of each node type, see [Node Reference](node-reference.md).

## Use the Properties Panel

Each node's configuration is available in a read-only properties panel. To inspect a node and open it for editing:

1. Click a node on the canvas. The properties panel opens on the right-hand side of the screen and displays the node's read-only configuration.
2. Review the fields for the selected node. For the fields displayed per node type, see [Node Reference](node-reference.md).
3. To edit the configuration, click **Open in Build**. The corresponding Build page record opens.
4. To close the panel, click its close button or click anywhere on the canvas background. You can also collapse the panel.

## Toolbar

The toolbar above the canvas displays the integration name and provides these controls:

* **Tidy Up** — Automatically rearranges the nodes into an optimized layout so large or complex diagrams are easier to read.
* **Export** — Exports the current diagram as an SVG, PNG, or JSON file. To export a diagram, complete the steps in [How to Export an Integration Diagram](../../how-to-guides/how-to-export-an-integration-diagram.md).
* **Close** — Returns to the integration selection screen without using the browser's back button.
