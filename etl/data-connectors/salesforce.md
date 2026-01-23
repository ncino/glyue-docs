# Salesforce

To connect to Salesforce, you'll need to set up an [External Client App](https://help.salesforce.com/s/articleView?id=xcloud.external_client_apps.htm\&type=5) in your environment. At the moment, the adapter only supports OAuth Client Credentials flow.

## Supported Tasks

* Extract
* Load

## Fields

<table><thead><tr><th width="150.88671875">Field</th><th width="300.51953125">Description</th><th>Example</th></tr></thead><tbody><tr><td>Host</td><td>The hostname of the Salesforce environment.</td><td><code>https://orgfarm-82910zd492-dev-ed.develop.my.salesforce.com</code></td></tr><tr><td>API Version</td><td>The version of the Salesforce API to use. By default, it uses <code>v64.0</code>.</td><td><code>v64.0</code></td></tr><tr><td>Client ID</td><td>The consumer key of the external client app.</td><td><code>APj7zFlNqDFtsNI1</code></td></tr><tr><td>Client Secret</td><td>The consumer secret of the external client app.</td><td><code>vloVg2IHG4wOUciqwWp7Sr6n0tg8s0Fc</code></td></tr><tr><td>Token Lifetime</td><td>How long, in seconds, until a new access token should be requested.<br><br>This setting should match the expiry setting configured for the external client app.<br><br>By default, it's set to 60 seconds.</td><td><code>60</code></td></tr></tbody></table>

