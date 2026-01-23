# Integration Builder Page

## Main Toolbar

Buttons on the Integration Builder Toolbar enables users to add, duplicate and clear changes within the Builder. The special tool, `deep clone` creates a full copy of the entire Builder Row selected, and all of the children associated with it.

<figure><img src="../.gitbook/assets/glyue_toolbar_20221024.png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
A new feature, [**Buildhelper**](../integration-gateway-platform-reference/buildhelper.md)**,** is now available to help create new Service Requests with pre-created fields for various external Systems / APIs.
{% endhint %}

***

## Filters

### Column

The column filter handles the display status of the Build Page. By default, all columns are shown but each column can be toggled to be hidden.

### Filter

The filter feature highlights matched Builder Rows based on the entered values. It will search in real-time and provide the number of results returned within the button.

### SQL Filter

This performs the back-half of a SQL filter for the table. This allows users to search with higher specificity than the traditional text based filter.

Example Filters

```
FROM * SELECT
```

### Comments

Comments are the main source of communication when jointly working within the Integration Builder. Comments can be created by anyone who has read-access to the integration.

### Tags

Each section of the Integration Builder has the ability to add a `tag` to Builder rows. This provides a way to identify specific pieces of an integration and how they relate. It is particularly helpful when used in combination with Bookmarks.



## Find and Replace

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

The Find and Replace toolbox offers a powerful way to quickly search for and update text within the cells on the build page. It will only search through visible cells.

1. The text to find.
2. The replacement text, if replacing.
3. Number of matches, by cell. Multiple instances of the text in one cell counts as one.
4. Move forward/backward through the matches. _Enter_ and _shift-Enter_ can also be used to move forward and backward respectively.
5. Replace the currently highlighted instance
6. Replace all found instances. If (11) _Replace within Selection_ is enabled, will only replace the matches within the selected cells.
7. When enabled, hides all rows that do not have a match.&#x20;
8. When enabled, looks for an exact case (case sensitive) match.
9. When enabled, only counts whole words as matches (e.g. "banker" will not match "bank").
10. When enabled, interprets the "find" text as a regular expression.
11. When enabled, _Replace All_ will only replace matches within the selected cells.&#x20;



***

### Integration Configuration Wizard

To create a new Integration, a wizard will take you through the required and optional configuration fields.

#### Step 1 — Configure Integration

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption><p>First page of the Integration Configure Wizard – "Integration Config"</p></figcaption></figure>

1. Name of the integration
2. Description for the integration
3. Whether services outside of Integration Gateway can trigger the integration
4. Whether the integration should run asynchronously
5. Send a detailed breakdown of every integration run alongside the Email Success / Failure messages (if enabled).
6. The list of emails to receive a notification when the integration finishes successfully.
   1. Multiple emails should be separated by a comma, e.g. `david@sandboxbanking.com, sarah@sandboxbanking.com`
7. The list of emails to receive a notification when the integration fails.
   1. Multiple emails should be separated by a comma
8. Whether or not Integration Gateway should [store the records of every run of this integration](integration_anatomy.md#run-history), and if so, for how long.
   1. Note storing the payloads for those runs are controlled by a separate setting (9)
9. Whether or not Integration Gateway should store the contents of the payloads of every run of this integration, and if so, for how long.
   1. Payloads will not be stored if the general run history storage (8) is not enabled
10. [Integration engine version](integration_components/integration-engine-versions.md), to preserve compatibility with platform functions as the platform evolves.&#x20;
    1. Integrations are developed against a specific Integration Engine version, so if unsure, consult Sandbox support before changing this setting.
11. [Webservice endpoints](web-service-endpoints.md), which allow an Integration to be triggered by user-defined URL paths.

#### Step 2 —Systems (adapters)

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption><p>Second page of the Integration Configure Wizard – "Systems"</p></figcaption></figure>

1. Clears all changes on the page.&#x20;
2. Saves changes to adapter bindings. When setting up a new Integration, any Systems added will need to be saved before you can `CONTINUE` to the next step
3. The system Integration Gateway will be connecting to
4. The configuration to use for the selected system
   1. If no configuration exists, you can create one using the _Add New Adapter_ option. This will open a popup with fields specific to the system you selected.&#x20;
   2. At a basic level, the adapter configuration contains the credentials for Integration Gateway to communicate with the system.
5. Adds additional systems, if the Integration requires multiple.

#### Step 3 — Permissions

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption><p>Third page of the Integration Configure Wizard – "Permissions"</p></figcaption></figure>

This page is used to set Read, Write, Execute, and Debug permissions for each user on the new integration. A description of each permission can be found at [#integration-permissions](../integration-gateway-platform-reference/integration_configuration.md#integration-permissions "mention").
