# Custom Adapters

The Custom Adapter feature lets you author an adapter in Python and store it outside any single integration, together with a typed configuration schema. You can reuse one custom adapter across integrations, and each adapter can hold several named configurations that each supply their own values.

Custom Adapters share the Shared Code page with [Shared Modules](../../shared-modules.md) and appear under the **Custom Adapters** tab.

## Permissions

Users must have the `can_use_custom_adapters` permission to access the Custom Adapters tab. This permission is independent of `can_use_shared_modules`; a user might have either, both, or neither.

## Anatomy

Users with the permission see the **Custom Adapters** tab on the Shared Code page.

A custom adapter comprises two kinds of object:

* A **Custom Adapter Code** — the Python source and its configuration schema. This is the reusable definition.
* One or more **Configs** — concrete sets of values that satisfy a Custom Adapter Code's schema. Each config belongs to exactly one Custom Adapter Code.

The tab contains these elements:

* **Adapter Selection Pane**: Select an existing custom adapter, or start a new one.
* **Adapter Name**: Name the adapter. The read-only **System ID** hint beneath this field shows the identifier that the platform derives from the name.
* **Description**: Enter an optional free-text description.
* **Adapter Code**: Enter the Python source for the adapter.
* **Config Schema**: Define the configuration fields and their types.
* **Save** and **Delete**: Save or remove the Custom Adapter Code.
* **Configs** table: Lists the configs defined for the adapter, their active state, and their validation status. This table appears after you save the adapter.

### Custom Adapter Code

<table><thead><tr><th>Field</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td><strong>REQUIRED</strong> | Unique display name for the adapter.</td></tr><tr><td>System ID</td><td><strong>DERIVED, READ-ONLY</strong> | Identifier used to reference the adapter. See <a href="#system-identifier">System Identifier</a>.</td></tr><tr><td>Description</td><td><strong>OPTIONAL</strong> | Free-text description.</td></tr><tr><td>Adapter Code</td><td><strong>REQUIRED</strong> | Python source for the adapter. See <a href="#adapter-code">Adapter Code</a>.</td></tr><tr><td>Config Schema</td><td><strong>REQUIRED</strong> | Field names and their types. See <a href="#config-schema">Config Schema</a>.</td></tr></tbody></table>

### System Identifier

The platform derives the System ID automatically from the name; you cannot edit it directly. To derive it, the platform replaces each run of non-alphanumeric characters with a single underscore, removes leading and trailing underscores, and uppercases the result.

<table><thead><tr><th>Name</th><th>System ID</th></tr></thead><tbody><tr><td><code>My REST API</code></td><td><code>MY_REST_API</code></td></tr><tr><td><code>acct-lookup (v2)</code></td><td><code>ACCT_LOOKUP_V2</code></td></tr></tbody></table>

The System ID must be unique across all custom adapters. If you save a name that derives to an identifier already in use, the platform rejects it with a validation error.

{% hint style="warning" %}
The platform recalculates the System ID from the name every time you save the adapter. Renaming an adapter changes its System ID, which can break existing references to the adapter.
{% endhint %}

### Config Schema

The config schema is a JSON object mapping each field name to a type. It defines which values a config for this adapter must supply.

<table><thead><tr><th>Type</th><th>Config value</th><th>Notes</th></tr></thead><tbody><tr><td><code>string</code></td><td>text</td><td></td></tr><tr><td><code>integer</code></td><td>whole number</td><td></td></tr><tr><td><code>float</code></td><td>number</td><td></td></tr><tr><td><code>boolean</code></td><td><code>true</code> / <code>false</code></td><td></td></tr><tr><td><code>array</code></td><td>JSON array</td><td>Enter as JSON in the config dialog.</td></tr><tr><td><code>object</code></td><td>JSON object</td><td>Enter as JSON in the config dialog.</td></tr><tr><td><code>encrypted_string</code></td><td>text</td><td>The platform encrypts this value at rest. See <a href="#encrypted-fields">Encrypted Fields</a>.</td></tr><tr><td><code>file</code></td><td>file upload</td><td>The platform encrypts this value at rest as a base64 string. Maximum size 1 MB. See <a href="#encrypted-fields">Encrypted Fields</a>.</td></tr></tbody></table>

Field names must be strings. The platform rejects any type outside this list when you save the adapter.

{% hint style="warning" %}
Changing the schema of an adapter that already has configs can invalidate those configs. Adding a field leaves existing configs without a value for it; changing a field's type might cause existing values to no longer match. The interface shows a warning describing the impact before you save such a change. Removing a field drops that field's value from existing configs.
{% endhint %}

## Configs

A config is a named set of values for one Custom Adapter Code. You enter a config through the config dialog. Each config has:

<table><thead><tr><th>Field</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td><strong>REQUIRED</strong> | Name of the config.</td></tr><tr><td>Active</td><td>Whether the config is available for use.</td></tr><tr><td>Config Values</td><td>One input per schema field, typed according to the schema.</td></tr></tbody></table>

A config's values must match the adapter's schema exactly: every schema field must be present, no config can supply an unlisted field, and each value must match its field's type. You cannot save a config that does not satisfy these rules.

### Encrypted Fields

The platform stores `encrypted_string` and `file` fields encrypted at rest, separately from the other config values.

In the config dialog and in API responses, the platform masks a stored encrypted value with the sentinel `__ENCRYPTED__` instead of showing it. When you edit an existing config:

* Leave the masked value in place to preserve the stored encrypted value.
* Enter a new value to replace it.

The config dialog shows `encrypted_string` fields as password inputs. A `file` field shows an "uploaded" state with a **Replace** action. The platform reads each uploaded file as base64, and the file must not exceed 1 MB.

## Adapter Code

The platform compiles and executes adapter code server-side. The Python editor performs syntax checking as you write the code.

### Available Names

The platform injects these names into the adapter code's namespace; you do not need to import them:

<table><thead><tr><th>Name</th><th>Description</th></tr></thead><tbody><tr><td><code>BaseAdapter</code></td><td>Base class the adapter class extends.</td></tr><tr><td><code>AdapterRequest</code></td><td>The request object passed to <code>execute()</code>.</td></tr><tr><td><code>AdapterResponse</code></td><td>The response object <code>execute()</code> returns.</td></tr><tr><td><code>AdapterFlowController</code></td><td>Flow-control helper for sequencing steps.</td></tr><tr><td><code>MessageTypes</code></td><td>Message-type constants used when reporting errors on a response.</td></tr><tr><td><code>base64</code></td><td>Python's <code>base64</code> module, for decoding <code>file</code> fields.</td></tr></tbody></table>

The standard `import` statement is available for other modules (for example `import requests`).

### Required Structure

Adapter code must define a class named exactly `CustomAdapter` that extends `BaseAdapter`:

```python
class CustomAdapter(BaseAdapter):
    def execute(self, request):
        ...
```

{% hint style="info" %}
If your code does not define a class named `CustomAdapter`, execution fails with the error "Custom adapter code must define a class named 'CustomAdapter'."
{% endhint %}

* `execute(self, request)` is **required**. It receives an `AdapterRequest` and must return an `AdapterResponse`.
* `validate_config(self)` is **optional**. The platform calls it during config validation; see [Config Validation](#config-validation).

### The Request Object

<table><thead><tr><th>Attribute</th><th>Description</th></tr></thead><tbody><tr><td><code>request.payload</code></td><td>The field mappings the integration passes to the adapter.</td></tr><tr><td><code>request.service_name</code></td><td>The name of the service request invoking the adapter.</td></tr></tbody></table>

### Access the Configuration

Inside the adapter, `self.config` is a plain dictionary of the resolved config values, with any encrypted fields already decrypted. Access values by key:

```python
host = self.config["base_url"]
timeout = self.config.get("timeout", 30)
```

`file` fields arrive as base64-encoded strings. Decode them with the injected `base64` module:

```python
content_bytes = base64.b64decode(self.config["my_file"])
content_text = content_bytes.decode("utf-8")  # for text files
```

### Return a Response

Build and return an `AdapterResponse`. Set `success` and `payload`, and report failures with `add_message_and_stack_trace(message_type, message)`:

```python
response = AdapterResponse()
response.payload = r.json()
response.success = True
return response
```

If the adapter does not handle an exception raised in `execute()`, the platform catches it and returns a failed `AdapterResponse` with the message type `MessageTypes.UNCATEGORIZED_ERROR`.

### Example: Default Template

New adapters open with the following template, which authenticates with a bearer token, posts the request payload, and maps common request failures onto message types:

```python
import requests


class CustomAdapter(BaseAdapter):
    """
    Custom REST adapter template.

    Config fields available via self.config (defined in Config Schema):
        - base_url: Base URL for the API
        - api_key: API key for authentication (encrypted)
        - timeout: Request timeout in seconds

    File fields are stored as base64 strings.
        content_bytes = base64.b64decode(self.config["my_file"])
        content_text = content_bytes.decode("utf-8")  # for text files

    Available imports (no need to import these):
        BaseAdapter, AdapterRequest, AdapterResponse,
        AdapterFlowController, MessageTypes, base64
    """

    def execute(self, request):
        response = AdapterResponse()
        response.success = False

        try:
            url = f"{self.config['base_url']}/{request.service_name}"
            headers = {
                "Authorization": f"Bearer {self.config['api_key']}",
                "Content-Type": "application/json",
            }

            r = requests.request(
                method="POST",
                url=url,
                headers=headers,
                json=request.payload,
                timeout=self.config.get("timeout", 30),
            )
            r.raise_for_status()

            response.payload = r.json()
            response.success = True

        except requests.exceptions.HTTPError as e:
            response.add_message_and_stack_trace(
                MessageTypes.from_status(e.response.status_code),
                f"{e.response.status_code}: {e.response.text}"
            )
        except requests.exceptions.ConnectionError as e:
            response.add_message_and_stack_trace(
                MessageTypes.CONNECTION_ERROR,
                str(e)
            )
        except requests.exceptions.Timeout as e:
            response.add_message_and_stack_trace(
                MessageTypes.CONNECTION_ERROR,
                f"Request timed out: {e}"
            )
        except Exception as e:
            response.add_message_and_stack_trace(
                MessageTypes.UNCATEGORIZED_ERROR,
                str(e)
            )

        return response

    def validate_config(self):
        if not self.config.get("base_url"):
            raise Exception("base_url is required")
        if not self.config.get("api_key"):
            raise Exception("api_key is required")
```

The template ships with a matching schema of `base_url` (string), `api_key` (encrypted_string), and `timeout` (integer).

## Config Validation

The **Validate** action on a single config, and **Validate All** on the adapter, instantiate the adapter with that config and call its `validate_config()` method:

* A `validate_config()` that returns without raising marks the config as passed.
* An exception from `validate_config()` marks the config as failed. View the raised message through the config's **View Log** action.
* A config whose code fails to compile fails validation with "Failed to compile custom adapter code: …".

Validation runs asynchronously; each config shows one of the following states:

<table><thead><tr><th>State</th><th>Meaning</th></tr></thead><tbody><tr><td>Spinner</td><td>Validation in progress.</td></tr><tr><td>Green check</td><td>Passed.</td></tr><tr><td>Red error icon</td><td>Failed. Open <strong>View Log</strong> for the message.</td></tr><tr><td>Amber warning</td><td>Result unknown. The validation process did not report a result.</td></tr><tr><td>—</td><td>Not yet validated.</td></tr></tbody></table>

## Custom Adapter Usage

A saved Custom Adapter Code's System ID identifies it. A config for that adapter must be active before the integration can use it. You invoke adapters from within an integration with the [`calladapter`](../../special_functions/calladapter.md) special function.

After the adapter is bound and active, it receives the integration's field mappings as `request.payload` and returns its `AdapterResponse` to the integration.

## Limitations and Notes

* Custom adapters provide no built-in authentication. You must implement any authentication a request requires in the adapter code, using values from the config schema. For example, store a token or password in an `encrypted_string` field. See [How to authenticate custom adapter requests](authenticate-custom-adapter-requests.md).
* Adapter code must define a class named exactly `CustomAdapter`.
* The platform derives the System ID from the name and changes it when you rename the adapter.
* Deleting a Custom Adapter Code also deletes all of its configs. Before you delete, the interface reports the number of configs it removes.
* Changing an adapter's config schema can invalidate existing configs. The interface warns you before you save such a change.
* `file` fields have a 1 MB limit.
* The platform stores `encrypted_string` and `file` values encrypted at rest and masks them in the interface and API.
