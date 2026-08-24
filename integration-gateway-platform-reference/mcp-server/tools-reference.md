# MCP Tools Reference

**Quick Reference:**

| Tool | Description | Read-Only |
|------|-------------|-----------|
| [`read_integration`](#read_integration) | Retrieve integration structure | Yes |
| [`write_integration`](#write_integration) | Create or update integrations | No |
| [`search_integration`](#search_integration) | Search for patterns in integration code | Yes |
| [`list_integrations`](#list_integrations) | List accessible integrations | Yes |
| [`execute_integration`](#execute_integration) | Execute an integration | No |
| [`get_run_history`](#get_run_history) | Retrieve run history for an execution | Yes |
| [`get_run_history_item`](#get_run_history_item) | Retrieve specific run history items | Yes |
| [`search_run_histories`](#search_run_histories) | Search and filter run histories | Yes |
| [`delete_integration_component`](#delete_integration_component) | Delete integrations or components | No |
| [`query_buildhelper`](#query_buildhelper) | Query integration build assistance | Yes |
| [`ig_platform_documentation`](#ig_platform_documentation) | Search the IG User Docs | Yes |
| [`frontend_operations`](#frontend_operations) | Build and manage frontends | No |
| [`django_admin`](#django_admin) | Manage Integration Gateway admin models | No |
| [`custom_adapter_code`](#custom_adapter_code) | Build and manage custom adapters | No |
| [`custom_adapter_config`](#custom_adapter_config) | Create and manage custom adapter configs | No |

---

### read_integration

Retrieves the complete or partial structure of an integration. Returns service requests, field mappings, validation rules, value mapping sets, and value mappings. Each component includes associated tags and comments.

**Returns:** Integration structure with the requested components and a warnings array for potential configuration issues.

**Behavior:**

- If you do not specify components, the tool returns all components
- You can selectively retrieve specific components
- Each integration includes a `last_saved` timestamp; pass it back on `write_integration` or `delete_integration_component` calls to detect merge conflicts

---

### write_integration

Creates or updates any part of an integration. The system automatically updates the integration's last saved timestamp. When you create an integration this way, the system automatically grants full permissions to the creator.

**Returns:** Object with success status, integration details, created_integration flag, detailed changes for each component (created/updated items with old/new values), and warnings array. On a merge conflict, returns `{"success": false, "error": "merge_conflict", "integration_name": ..., "current_last_saved": ..., "message": "Your information is out of date..."}` instead.

**Behavior:**

- Include the `id` field to update existing components
- Omit the `id` field to create new components
- New child components must include a parent ID (`servicerequest_id`, `valuemappingset_id`)
- Set `create_if_missing` to `true` to create the integration if it does not exist
- When you update an existing integration, include the `last_saved` timestamp from a prior `read_integration` call to detect merge conflicts. If another user has saved changes since, the request fails with a `merge_conflict` error and does not overwrite them.
- Merge-conflict detection does not apply to integration creation

---

### search_integration

Searches for a regex pattern across all code fields in an integration. You can specify search components to limit the scope, or search all components by default. Case sensitivity defaults to `true`. The number of context lines before and after matches defaults to 2.

**Returns:** Object with integration details, total match count, and matches organized by component type.

---

### list_integrations

Lists all integrations accessible to the user with their permission levels. Can optionally filter to show only active integrations.

**Returns:** Object with total count and array of integrations with a permissions object (read, write, execute, debug booleans).

---

### execute_integration

Executes an integration directly. If you provide a `tool_definition` and the integration has no registered MCP tool, the system automatically registers one. You can provide the payload directly or extract it from a previous run history.

**Returns:** Result varies by scenario:

- **Not registered, no tool_definition**: Object with `success: false`, `requires_tool_definition: true`, and guidance message
- **Validation fails**: Object with `success: false` and `validation_errors` array
- **Success**: Object with `success: true`, integration details, result data, and optional `run_history_id`
- **Failure**: Object with `success: false`, error message, and error type

**Behavior:**

- You must restart the client to see a newly registered tool in the tools list
- `payload` and `run_history_id` are mutually exclusive input methods

---

### get_run_history

> Requires the **Can use MCP Run History tool** Django user permission.

Retrieves detailed run history for a specific integration execution. You can optionally include or exclude items, messages, and labels. All default to included.

**Returns:** Object with run history metadata, optional labels array, and items overview array with size estimates. If total size exceeds 900 KB, the tool returns a `size_error` instead of full items.

**Behavior:**

- The tool checks estimated item sizes and prevents response overflow automatically
- Use `get_run_history_item` for items that exceed the 900 KB threshold

---

### get_run_history_item

> Requires the **Can use MCP Run History tool** Django user permission.

Retrieves run history items with optional search through regex or jq expressions.

**Modes:**

- Get a specific item by `item_id`
- Search within an item using `item_id` + `search`
- Search across run history using `run_history_id` + `search`

**Returns:** Full item with request, response, content, stack trace, and messages. If an item exceeds 900 KB, returns an error object with a suggestion to use search. Search results include match arrays with item IDs and types.

---

### search_run_histories

> Requires the **Can use MCP Run History tool** Django user permission.

Searches and filters run histories for integrations. This tool supports all filter methods available in the web interface.

**Returns:** Object with record count, records per page limit, and array of run history records with metadata. Text searches also include a search report and match item IDs.

**Behavior:**

- Returns only the first page of results ordered by most recent first
- Datetime filters accept ISO 8601 format, and the system automatically converts timezones to UTC

---

### delete_integration_component

Deletes an integration or component from Integration Gateway. The AI assistant must ask the user for explicit confirmation before it calls this tool.

**Returns:** Object with success status, integration details, deleted component details, and confirmation message.

**Behavior:**

- The tool description instructs the AI to request explicit user confirmation before it executes a delete
- When you delete an integration, the system deletes all of its components (service requests, field mappings, validation rules, value mapping sets, and value mappings)
- Include the `last_saved` timestamp from a prior `read_integration` call to detect merge conflicts. If another user has saved changes since, the request fails with a `merge_conflict` error and does not delete stale data.

---

### query_buildhelper

Queries the Build Helper service for integration building assistance. Use this tool to discover available services, adapters, and pre-built field mapping templates.

**Endpoints:**

- `list_all_services`: Get complete catalog of services with IDs
- `list_all_adapters`: Get complete catalog of adapters with IDs
- `list_services_for_adapter`: Get catalog of services for a specific adapter
- `get_field_mapping_sets`: Get pre-built templates for a specific service
- `get_field_mappings`: Get actual field mappings from a template

**Typical Workflow:**

1. `list_all_adapters` to get the adapter catalog
2. `list_services_for_adapter` to get services for a specific adapter
3. `get_field_mapping_sets` to get templates for a service
4. `get_field_mappings` to get field mappings from a template

---

### ig_platform_documentation

Searches the Integration Gateway User Docs to answer questions about platform features and usage. You can also retrieve a specific documentation page directly.

**Actions:**

- `ask`: Ask a natural-language `question`, optionally with a `goal` for added context
- `get_page`: Retrieve a specific documentation page by `url`

**Returns:** Relevant documentation content that answers the question, or the full content of the requested page.

---

### frontend_operations

Performs CRUD operations on Integration Gateway frontends. Frontends enforce Content Security Policy (CSP) restrictions: HTML files must not contain inline script tags or inline event handlers. All JavaScript must be in separate `.js` files.

**Actions:**

- `list-frontends`: List all available frontends with names, paths, URLs, and allowed domains
- `create-frontend`: Create a new frontend with a CSP-compliant template (index.html, styles.css, script.js)
- `list-files`: List all files in a frontend's zip archive
- `get-file`: Get content of specific files from a frontend zip
- `put-file`: Create or update files in a frontend zip
- `delete-file`: Delete files from a frontend zip

**Behavior:**

- File paths cannot be absolute, contain null bytes, or use `..` path traversal
- The system auto-generates the frontend path from the name

---

### django_admin

Manages Django admin-registered models in Integration Gateway. Use this tool to list available models, retrieve instances, create instances, update instances, and delete instances. All operations respect Django admin permissions and generate audit log entries.

**Actions:**

- `describe-models`: List all admin-registered models the user can access, with permissions and field metadata
- `get-model`: Retrieve one or more model instances with optional Django ORM filtering
- `post-model`: Create a new model instance with form validation
- `patch-model`: Update an existing model instance (only specified fields change)
- `delete-model`: Delete a model instance (requires explicit user confirmation)

**Behavior:**

- Model names must use `app_label.ModelName` format
- Foreign key fields use the `_id` suffix with an integer value or null
- Many-to-many fields accept a list of integer IDs
- The system masks encrypted fields in output
- The system blocks certain models (for example, GlobalConfig) from access
- The system caps results at 200 instances per query

---

### custom_adapter_code

> Requires the **can_use_custom_adapters** account permission.

Manages custom adapter code definitions. Custom adapters are user-defined Python classes that inherit from `BaseAdapter`. Each adapter defines a configuration schema specifying the fields its instances accept.

**Actions:**

- `list`: List all adapters. Code is omitted by default; set `include_code` to `true` to include it
- `get`: Retrieve a single adapter by ID. Includes code by default
- `create`: Create an adapter. Requires `name` and `code`; optionally accepts `description` and `config_schema`
- `update`: Update an adapter. Requires `id` plus at least one of `name`, `description`, `code`, or `config_schema`
- `delete`: Delete an adapter by ID. Cascade-deletes all associated configs
- `deletion_impact`: Report how many configs the system would cascade-delete
- `schema_change_impact`: Report warnings for a proposed schema change (added fields, type changes)

**Returns:** Adapter object (ID, name, system, description, config_schema, timestamps, and optionally code) for get/create/update. Count and array for list. Impact details for the impact actions. Success confirmation for delete.

**Behavior:**

- Adapter code must define a `CustomAdapter` class inheriting from `BaseAdapter` with an `execute(self, request)` method. The `validate_config(self)` method is optional
- Pre-loaded imports: `BaseAdapter`, `AdapterRequest`, `AdapterResponse`, `AdapterFlowController`, `MessageTypes`, `base64`
- Access config values through `self.config`; file fields are base64 strings and must be decoded before use
- Schema field types must be one of: `string`, `integer`, `float`, `boolean`, `array`, `object`, `encrypted_string`, `file`
- Removing schema fields also removes them from all existing configs

---

### custom_adapter_config

> Requires the **can_use_custom_adapters** account permission.

Manages configurations for a custom adapter. Each config stores values matching the parent adapter's schema and represents a specific instance the adapter can run with.

**Actions:**

- `list`: List all configs for an adapter. Requires `code_id`
- `get`: Retrieve a single config. Requires `code_id` and `config_id`
- `create`: Create a config. Requires `code_id` and `name`; optionally accepts `active` (defaults to true) and `config_values`
- `update`: Update a config. Requires `code_id` and `config_id` plus at least one of `name`, `active`, or `config_values`
- `delete`: Delete a config. Requires `code_id` and `config_id`
- `validate`: Run validation on all active, validation-enabled configs for the adapter and return results
- `validation_result`: Return results from the most recent validation run without re-running

**Returns:** Config object (ID, name, active, config_values) for get/create/update. Count and array for list. Results array (config_id, config_name, success, message, timestamp) for validate/validation_result. Success confirmation for delete.

**Behavior:**

- The system coerces values to schema types automatically
- File fields accept base64 strings up to 1 MB decoded
- The system masks encrypted and file fields in responses; they cannot be read back
- Omit encrypted and file fields on update to preserve existing values

---

## User-Defined Tools

In addition to the built-in tools, Integration Gateway supports user-defined tools that execute specific integrations. Administrators create these tools through the Integration Gateway admin interface.

Each user-defined tool:

- Executes a specific integration
- Has a custom name and description
- Has a custom input schema (JSON Schema)
- Accepts a payload that conforms to its input schema
- Returns the result of the integration execution

You can also register integrations as MCP tools programmatically using the `execute_integration` tool with a `tool_definition` parameter.
