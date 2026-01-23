# HubSpot

Unlike other Data Connectors, HubSpot uses OAuth authentication to obtain account credentials. If you need more control over the various scopes used, consider using a [Private App](https://developers.hubspot.com/docs/apps/legacy-apps/private-apps/overview).

## Supported Tasks

* Extract
* Load

## Fields

<table><thead><tr><th width="147.34375">Field</th><th width="306.6015625">Description</th><th>Example</th></tr></thead><tbody><tr><td>Date Format</td><td>The date format that uploaded dates should be in.<br><br>Currently, only a few date formats are supported: <code>MM/DD/YYYY</code>, <code>YYYY-MM-DD</code>, and <code>DD/MM/YYYY</code>.<br><br>HubSpot requires that all dates adhere to the same format when uploading.</td><td><code>MM/DD/YYYY</code></td></tr><tr><td>Use Private App</td><td>A toggle specifying whether to use built-in OAuth authentication or a manually-specified access token obtained via a Private App.</td><td><i class="fa-check">:check:</i></td></tr><tr><td>Access Token</td><td>The access token associated with a Private App.<br><br>Only required when "Use Private App" is checked.<br><br>For more information on Private Apps, check out <a href="https://developers.hubspot.com/docs/apps/legacy-apps/private-apps/overview#make-api-calls-with-your-app%E2%80%99s-access-token">HubSpot's documentation</a>.</td><td><code>COP3N1zcIdcTfbDypwk7mcNhxIflWuT6</code></td></tr></tbody></table>
