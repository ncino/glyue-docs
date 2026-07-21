# Workbench Node Reference

The Workbench canvas represents an integration as a diagram of connected nodes. A color, label, and icon identify each node type. When you click a node, the system opens a read-only properties panel that displays the configuration for that node.

This page describes each node type and the fields shown in its properties panel. For canvas navigation and general use, see [Integration Gateway Workbench](README.md).

Every properties panel includes an **Open in Build** link that navigates to the corresponding Build page record.

## Node Types

The canvas displays four node types. Edges connect the nodes to represent the integration's execution flow:

| Node | Represents |
| --- | --- |
| **Integration** | The root node for the integration. |
| **Service Request** | Each service request within the integration. |
| **Field Mapping** | A field mapping attached to a service request. |
| **Validation Rule** | A validation rule attached to a service request or to the integration. |

A **Value Mapping Set** does not appear as its own node. When a field mapping or validation rule references a value mapping set, that set renders within the node's properties panel.

## Integration Node

The properties panel header reads **Integration**. The panel displays the integration's configuration as read-only fields:

* **Path Name**
* **Description**
* **Active**
* **Run Async**
* **HTTP API**
* **Tags**

Integration-level hooks display in a collapsible section: Before Hook, On Success Hook, On Failure Hook, Finally Hook, Swagger Response, and Swagger Request.

### Adapter Details

The **Adapter Details** section lists the adapters bound to the integration, each with its associated configuration fields. All fields are read-only.

## Service Request Node

The properties panel header reads **Service Request**. The panel displays these read-only fields:

* **Service Name**
* **Formula Variable** — The panel shows this above the service name.
* **System**
* **Sequence**
* **Active**
* **Abort After Execute Request Failure**
* **Skip Execute If No Sub Requests**
* **Message Substitution Name**
* **Notes**
* **Tags**

### Hooks

Configured hooks display in a collapsible section, each listed by name:

* **Before Prepare Request Hook**
* **After Prepare Request Success Hook**
* **After Prepare Request Failure Hook**
* **Before Execute Request Hook**
* **After Execute Request Success Hook**
* **After Execute Request Failure Hook**
* **After Overall Success Hook**
* **After Overall Failure Hook**

### Logic Conditions

Conditional and iteration logic displays in a collapsible section: Call If and Call For Each.

## Field Mapping Node

The properties panel header reads **Field Mapping**. The panel displays these read-only fields:

* **Field**
* **Value**
* **Value Type**
* **Sequence**
* **Active**
* **Nullable**
* **Source Record Type**
* **Source Field Name**
* **Target Record Type**
* **Target Field Name**
* **Message Substitution Name**
* **Notes**
* **Tags**

Conditional and iteration logic displays in a collapsible section: Include If and Include For Each.

### Value Mapping Set

When a field mapping references a value mapping set, the **Value Mapping Set** sub-section displays its name and a scrollable table of the value mappings in the set.

## Validation Rule Node

The properties panel header reads **Validation Rule**. A validation rule can attach to either a Service Request node or an Integration node, depending on its configuration. The panel displays these read-only fields:

* **Type**
* **Field**
* **Rule**
* **Sequence**
* **Active**
* **Abort On Failure**
* **Message Substitution Name**
* **Notes**
* **Tags**

Conditional and iteration logic displays in a collapsible section: Apply If, Apply To Each, Pass Hook, and Fail Hook.

When a validation rule references a value mapping set, its details display in the panel in the same way as for a field mapping.
