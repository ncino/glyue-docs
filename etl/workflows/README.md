# Workflows

A Workflow represents an ETL flow. In most cases, a workflow will consist of the following:

* Extraction from a source connection
* Transformation of the extracted data
* Upload into a destination connection

## Workflow Editor

The Workflow Editor is the interface where you build your workflow. It's a node-based editor where you describe the data flow by adding and connecting various nodes.

<figure><img src="../../.gitbook/assets/ETL Workflow Editor (2).png" alt=""><figcaption></figcaption></figure>

In the image above, you can see an example of a complete workflow. Here's a general overview of the various aspects of the interface:

1. The name of the workflow.
2. The environment the workflow is being viewed in.
3. An Extract node, where the source data originates from.
4. A Transform node, where source columns are modified and/or mapped to new columns.&#x20;
5. A Load node, where the modified data is loaded into.
6. Node options (e.g., rename, delete, etc.).
7. Node connection points and edges.
8. Button to access workflow settings.
9. Button to access the workflow scheduler.

{% hint style="info" %}
At the moment, each node can only have one incoming connection (if available) and one outgoing connection (if available).
{% endhint %}

### Settings

Beyond configuring the individual nodes that make up a workflow, each workflow has various settings that can be configured.

<figure><img src="../../.gitbook/assets/ETL Workflow Settings.png" alt=""><figcaption></figcaption></figure>

## Nodes

For more information about the individual nodes, please refer to their specific reference pages.

{% content-ref url="extract.md" %}
[extract.md](extract.md)
{% endcontent-ref %}

{% content-ref url="transform.md" %}
[transform.md](transform.md)
{% endcontent-ref %}

{% content-ref url="load.md" %}
[load.md](load.md)
{% endcontent-ref %}

{% content-ref url="filter.md" %}
[filter.md](filter.md)
{% endcontent-ref %}
