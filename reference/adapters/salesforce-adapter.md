# Salesforce Adapter

The Salesforce adapter facilitates communication with Salesforce from Integration Gateway (IG). Currently, the adapter supports Salesforce's REST API, Bulk API 1.0, and Bulk API 2.0.

{% hint style="info" %}
For authentication, it's recommended to use an External Client App. For information on External Client Apps, see [the Salesforce documentation](https://help.salesforce.com/s/articleView?id=xcloud.external_client_apps.htm\&type=5).
{% endhint %}

## Adapter Config

<table><thead><tr><th width="141.86328125">Field</th><th width="367.41015625">Description</th><th>Example</th></tr></thead><tbody><tr><td>Host</td><td><strong>REQUIRED</strong> | The domain of a Salesforce org.</td><td><code>my-org.my.salesforce.com</code></td></tr><tr><td>Authentication Type</td><td><strong>REQUIRED</strong> | The method of authenticating with Salesforce.<br><br>Available methods are: Client Credentials (recommended) and Password.</td><td><code>Client Credentials</code></td></tr><tr><td>Client ID</td><td><strong>REQUIRED</strong> | The Customer Key for an External Client App.</td><td><code>3MVG9dAEux2v1sLu1nEMEi2A0gJw6yQyZTbXuAUzDT7yRSe5R05pzQ.UR3bBAemuOCEctQ5ppm5YK5Z4laKCj</code></td></tr><tr><td>Client Secret</td><td><strong>REQUIRED</strong> | The Customer Secret for an External Client App.</td><td><code>16D1B629C81A9D002243906E3F8D5DF36841CD770DA8F00C0C0A663A6CD6DE6D</code></td></tr><tr><td>Username</td><td><strong>OPTIONAL</strong> | The username of a Salesforce user.<br><br>This is only required when "Authentication Type" is <code>Password</code>.</td><td><code>my_username</code></td></tr><tr><td>Password</td><td><strong>OPTIONAL</strong> | The password for a Salesforce user.<br><br>This is only required when "Authentication Type" is <code>Password</code>.</td><td><code>my_password</code></td></tr><tr><td>Salesforce API Version</td><td><strong>REQUIRED</strong> | The version of the Salesforce API to use for REST API calls.<br><br>Defaults to <code>v45.0</code>.</td><td><code>v45.0</code></td></tr><tr><td>Bulk Upload Poll Interval (seconds)</td><td><strong>REQUIRED</strong> | The interval at which Bulk Uploads are polled.<br><br>Defaults to <code>2</code> seconds.</td><td><code>2</code></td></tr><tr><td>Bulk Upload API Version</td><td><strong>REQUIRED</strong> | The version of the Salesforce API to use for Bulk API uploads.<br><br>Defaults to <code>v45.0</code>.</td><td><code>v45.0</code></td></tr><tr><td>Bulk Query Poll Interval (seconds)</td><td><strong>REQUIRED</strong> | The interval at which Bulk Queries are polled.<br><br>Defaults to <code>2</code> seconds.</td><td><code>2</code></td></tr><tr><td>Bulk Query API Version</td><td><strong>REQUIRED</strong> | The version of the Salesforce API to use for Bulk API queries.<br><br>Defaults to <code>v45.0</code>.</td><td><code>v45.0</code></td></tr><tr><td>Connection Timeout (seconds)</td><td><strong>REQUIRED</strong> | The number of seconds before an API call times out.<br><br>Defaults to <code>2100</code>.</td><td><code>2100</code></td></tr></tbody></table>

## Field Mappings

The Field Mappings for the Salesforce adapter differ depending on the Service Name passed to the Service Request.

Currently, the following Service Names are supported: `BULK_UPLOAD` and `BULK_QUERY` for Bulk API 1.0, `BULK_UPLOAD_2` and `BULK_QUERY_2` for Bulk API 2.0, and anything else for the REST API.

{% hint style="info" %}
Any non-adapter-specific Field Mappings are automatically included in the request body.
{% endhint %}

### REST API

Service Name: Any

<table><thead><tr><th width="165.359375">Field</th><th width="308.82421875">Description</th><th>Example</th></tr></thead><tbody><tr><td><code>method</code></td><td><strong>REQUIRED</strong> | The HTTP method.<br><br>Any of: <code>GET</code>, <code>POST</code>, <code>PUT</code>, <code>PATCH</code>, or <code>DELETE</code>.</td><td><code>GET</code></td></tr><tr><td><code>url</code></td><td><strong>REQUIRED</strong> | The REST API endpoint.<br><br>The endpoint is appended to the <code>host</code> specified in the adapter config.</td><td><code>/services/data/v67.0/sobjects/Account/</code></td></tr><tr><td><code>allOrNone</code></td><td><strong>OPTIONAL</strong> | A flag indicating that, for a composite request, all sub-requests must succeed; otherwise, all requests are rolled back.</td><td><code>false</code></td></tr></tbody></table>

### Bulk API 1.0

Service Name: `BULK_QUERY`

<table><thead><tr><th width="108.34765625">Field</th><th width="372.28515625">Description</th><th>Example</th></tr></thead><tbody><tr><td><code>operation</code></td><td><strong>REQUIRED</strong> | The type of operation to perform.<br><br>The following operations are supported: <code>query</code> and <code>queryAll</code>.</td><td><code>query</code></td></tr><tr><td><code>q</code></td><td><strong>REQUIRED</strong> | The SOQL query.</td><td><code>SELECT Id, Name FROM Account</code></td></tr><tr><td><code>object</code></td><td><strong>REQUIRED</strong> | The Salesforce object being queried.</td><td><code>Account</code></td></tr></tbody></table>

Service Name: `BULK_UPLOAD`

<table><thead><tr><th width="111.20703125">Field</th><th width="415.9296875">Description</th><th>Example</th></tr></thead><tbody><tr><td><code>operation</code></td><td><strong>REQUIRED</strong> | The type of operation to perform.<br><br>The following operations are supported: <code>insert</code>, <code>update</code>, <code>upsert</code>, <code>delete</code>, and <code>hardDelete</code>.</td><td><code>upsert</code></td></tr><tr><td><code>object</code></td><td><strong>REQUIRED</strong> | The Salesforce object being inserted, updated, or deleted.</td><td><code>Account</code></td></tr><tr><td><code>headers</code></td><td><strong>REQUIRED</strong> | A list of strings, where each string represents a field/column on the Salesforce object.</td><td><code>["Id", "Name"]</code></td></tr><tr><td><code>records</code></td><td><strong>REQUIRED</strong> | A list of lists, where each sublist contains the field values for a record of the Salesforce object in the order specified by <code>headers</code>.</td><td><code>[[1, "John Doe"]]</code></td></tr></tbody></table>

### Bulk API 2.0

Service Name: `BULK_QUERY_2`

<table><thead><tr><th width="108.34765625">Field</th><th width="372.28515625">Description</th><th>Example</th></tr></thead><tbody><tr><td><code>operation</code></td><td><strong>REQUIRED</strong> | The type of operation to perform.<br><br>The following operations are supported: <code>query</code> and <code>queryAll</code>.</td><td><code>query</code></td></tr><tr><td><code>q</code></td><td><strong>REQUIRED</strong> | The SOQL query.</td><td><code>SELECT Id, Name FROM Account</code></td></tr></tbody></table>

Service Name: `BULK_UPLOAD_2`

<table><thead><tr><th width="111.20703125">Field</th><th width="415.9296875">Description</th><th>Example</th></tr></thead><tbody><tr><td><code>operation</code></td><td><strong>REQUIRED</strong> | The type of operation to perform.<br><br>The following operations are supported: <code>insert</code>, <code>update</code>, <code>upsert</code>, <code>delete</code>, and <code>hardDelete</code>.</td><td><code>upsert</code></td></tr><tr><td><code>object</code></td><td><strong>REQUIRED</strong> | The Salesforce object being inserted, updated, or deleted.</td><td><code>Account</code></td></tr><tr><td><code>headers</code></td><td><strong>REQUIRED</strong> | A list of strings, where each string represents a field/column on the Salesforce object.</td><td><code>["Id", "Name"]</code></td></tr><tr><td><code>records</code></td><td><strong>REQUIRED</strong> | A list of lists, where each sublist contains the field values for a record of the Salesforce object in the order specified by <code>headers</code>.</td><td><code>[[1, "John Doe"]]</code></td></tr></tbody></table>
