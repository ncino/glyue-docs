# MCP Server

The Integration Gateway Model Context Protocol (MCP) server enables AI assistants to interact with integrations directly from any MCP-compatible client. It connects to your Integration Gateway instance and provides tools to read, write, execute, and debug integrations.

Key capabilities:

- Read and search integration structures, such as service requests, field mappings, and validation rules
- Create, update, and delete integrations and their components
- Execute integrations and inspect run history
- Access system documentation and build helper services

**Server type:** Streamable HTTP
**Protocol version:** 2025-06-18

---

## Prerequisites

Before you begin, you need:

- **Node.js**: v21 or later (includes npx). Download from [nodejs.org](https://nodejs.org/). If you upgrade from an older version, use [nvm](https://github.com/nvm-sh/nvm#installing-and-updating) on Mac/Linux or run the new installer on Windows.
- **mcp-remote**: The configuration examples in this guide use `mcp-remote@0.1.29`. This version is tested and verified with Integration Gateway. Do not substitute a newer version without confirming compatibility.
- **Integration Gateway access**: Staff or Superuser access on your Integration Gateway instance.
- **OAuth 2.0 credentials**: The setup process in [Configuration](#configuration) generates these automatically. If you need to create them manually, see [Manual OAuth Setup](#manual-oauth-setup).

---

## How It Works

1. Your MCP client launches a local `mcp-remote` proxy process with npx.
2. The proxy connects to your Integration Gateway instance over Streamable HTTP.
3. Integration Gateway triggers an OAuth 2.0 authorization flow in your browser.
4. After you authorize, the server presents available tools to the AI assistant.
5. When you ask a question or give a command, the AI selects the appropriate tool and sends a request through the proxy to Integration Gateway.
6. Integration Gateway executes the request and returns the results to your AI assistant.

---

## Configuration

### Step 1: Generate Your MCP Configuration

1. Navigate to your Integration Gateway admin page.
2. Select **MCP Tools**.
3. Expand the first-time setup instructions at the top of the page and follow the prompts.

The setup generates a configuration block with your OAuth credentials and instance URL. Copy this configuration for Step 2.

> **Manual setup:** If the self-service setup fails, see [Manual OAuth Setup](#manual-oauth-setup) in the Troubleshooting section.

### Step 2: Configure Your MCP Client

**Claude Desktop** — paste the generated configuration into `claude_desktop_config.json` (Settings > Developer > Edit Config), then restart Claude Desktop completely.

**Claude Code** — run this command in your terminal, replacing the placeholder values with the generated configuration:

```shell
claude mcp add --scope user --transport stdio IG-MCP -- \
  npx mcp-remote@0.1.29 \
  "https://your-ig-instance.com/integrations/mcp/" \
  "6545" \
  --transport http-only \
  --static-oauth-client-info \
  '{"client_id":"your_client_id","client_secret":"your_client_secret"}'
```

To scope the server to a single project instead, replace `--scope user` with `--scope project` or omit it for local scope.

Replace these values:

| Placeholder | Description |
|-------------|-------------|
| `your-ig-instance.com` | Your Integration Gateway base URL |
| `your_client_id` | Your OAuth client ID from the generated configuration |
| `your_client_secret` | Your OAuth client secret from the generated configuration |

> **Local development instances:** If your Integration Gateway instance runs on `http://localhost`, add `"--allow-http"` to the args array after `"http-only"`.

### Environment Variables

This MCP server does not use environment variables. Pass authentication credentials as command-line arguments through the `--static-oauth-client-info` flag.

---

## What You Can Do

Use natural language prompts with your AI assistant to work with Integration Gateway. These examples show common tasks organized by category.

### Find Information

| What to Ask | What Happens |
|-------------|--------------|
| "Show me the structure of the [integration name] integration" | Retrieves the complete integration structure including service requests, field mappings, and validation rules |
| "List all integrations I have access to" | Returns all accessible integrations with your permission levels |
| "Search for [pattern] across all code fields in [integration name]" | Searches integration code using regex pattern matching |
| "Show me the run history for [integration name]" | Retrieves execution history with metadata, labels, and item details |

### Create or Update

| What to Ask | What Happens |
|-------------|--------------|
| "Create a new integration called [name]" | Creates a new integration and grants you full permissions |
| "Add a new service request to [integration name]" | Creates a new component within an existing integration |
| "Update the field mapping [name] in [integration name]" | Modifies an existing component with new values |
| "Delete the [component] from [integration name]" | Removes a specific component or entire integration |

### Execute and Debug

| What to Ask | What Happens |
|-------------|--------------|
| "Execute the [integration name] integration with this payload: [data]" | Runs the integration and returns the result |
| "Show me the details of run history [ID]" | Retrieves detailed execution data including request/response content |
| "Search run histories for [integration name] that failed" | Filters run history records by status, date, or text content |

### Build and Explore

| What to Ask | What Happens |
|-------------|--------------|
| "Show me the system documentation for [topic]" | Searches and retrieves Integration Gateway documentation |
| "List all available adapters" | Returns the catalog of adapters from the Build Helper service |
| "Get field mapping templates for [service]" | Retrieves pre-built templates for a specific service |
| "Create a new frontend called [name]" | Creates a new CSP-compliant frontend with boilerplate files |

---

## Tools Reference

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
| [`deployed_integrations`](#deployed_integrations) | Search and retrieve deployed integrations (nCino only) | Yes |
| [`system_documentation`](#system_documentation) | Access system documentation (nCino only) | Yes |
| [`query_buildhelper`](#query_buildhelper) | Query integration build assistance | Yes |
| [`frontend_operations`](#frontend_operations) | Build and manage frontends | No |
| [`django_admin`](#django_admin) | Manage Integration Gateway admin models | No |

---

### read_integration

Retrieves the complete or partial structure of an integration. Returns service requests, field mappings, validation rules, value mapping sets, and value mappings. Each component includes associated tags and comments.

**Returns:** Integration structure with the requested components and a warnings array for potential configuration issues.

**Behavior:**

- If you do not specify components, the tool returns all components
- You can selectively retrieve specific components

---

### write_integration

Creates or updates any part of an integration. The system automatically updates the integration's last saved timestamp. When you create an integration this way, the system automatically grants full permissions to the creator.

**Returns:** Object with success status, integration details, created_integration flag, detailed changes for each component (created/updated items with old/new values), and warnings array.

**Behavior:**

- Include the `id` field to update existing components
- Omit the `id` field to create new components
- New child components must include a parent ID (`servicerequest_id`, `valuemappingset_id`)
- Set `create_if_missing` to `true` to create the integration if it does not exist

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

---

### deployed_integrations

> **nCino internal only.** This tool is available only to nCino employees and does not appear for external users.

Searches and retrieves deployed integrations.

**Actions:**

- `search`: Search deployed integrations
- `get_integration`: Get full integration with service requests and payloads
- `get_service_request`: Get full service request with payloads

**Returns:** Deployed integration data based on action. Search returns matching integrations with metadata. Other actions return full integration or service request data with payloads.

---

### system_documentation

> **nCino internal only.** This tool is available only to nCino employees and does not appear for external users.

Searches and retrieves Integration Gateway system documentation.

**Actions:**

- `search`: Search documentation (the system returns metadata only, not content)
- `get_page`: Get full documentation page content

**Behavior:**

- The search action provides only metadata
- Use `get_page` with the `page_id` from search results to retrieve full content

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

## User-Defined Tools

In addition to the built-in tools, Integration Gateway supports user-defined tools that execute specific integrations. Administrators create these tools through the Integration Gateway admin interface.

Each user-defined tool:

- Executes a specific integration
- Has a custom name and description
- Has a custom input schema (JSON Schema)
- Accepts a payload that conforms to its input schema
- Returns the result of the integration execution

You can also register integrations as MCP tools programmatically using the `execute_integration` tool with a `tool_definition` parameter.

---

## Permissions and Security

### Authentication

Integration Gateway uses OAuth 2.0 for authentication. When you first connect, your browser opens an authorization flow. After you authorize, the MCP proxy caches the token for later requests.

To set up credentials, complete the steps in [Configuration](#configuration) to generate your OAuth credentials and configure your MCP client.

### Access Controls

MCP tools respect your existing Integration Gateway permissions. You can only access integrations and data that your account has permission to use.

| Permission | What It Grants | How to Configure |
|------------|----------------|------------------|
| **Staff or Superuser** | Access to the MCP server and tools | Assign in Integration Gateway user management |
| **Integration read** | Read integration structures and search code | Set per-integration in Integration Gateway |
| **Integration write** | Create, update, and delete integration components | Set per-integration in Integration Gateway |
| **Integration execute** | Run integrations and user-defined tools | Set per-integration in Integration Gateway |
| **Integration debug** | Access run history data | Set per-integration in Integration Gateway |
| **Can use MCP Run History tool** | Access to `get_run_history`, `get_run_history_item`, and `search_run_histories` | Assign as a Django user permission |

### Data Access Scope

- **Can read**: Integration structures, run history, system documentation, deployed integrations, and Django admin models you have permission to access
- **Can modify**: Integrations, integration components, frontends, and Django admin models you have write permission for
- **Cannot access**: Integrations without granted permissions, blocked admin models (for example, GlobalConfig), and encrypted field values (the system masks these in output)

---

## Troubleshooting

### Verify the Connection

1. Look for the MCP server indicator in your client's interface (for example, the hammer icon in Claude Desktop).
2. Confirm the Integration Gateway MCP tools appear in your available tools list.
3. Ask your AI assistant to list integrations to confirm the connection works.

### Common Errors

**"spawn npx ENOENT"**

Your system cannot find npx. Use the full path instead.

**Fix:**

1. Find your npx location:

```shell
which npx
```

2. Update your config to use the full path:

```json
{
  "mcpServers": {
    "IG-MCP": {
      "command": "/opt/homebrew/bin/npx",
      "args": [...]
    }
  }
}
```

3. Restart your MCP client completely.

---

**"SyntaxError: The requested module 'node:fs/promises'"**

Your Node.js version is too old. Upgrade to v21 or later.

**Fix:**

```shell
# Check current version
node --version

# If you use nvm, switch versions
nvm use 22
nvm alias default 22
```

Then update your config to use the newer Node.js npx path.

---

**"Incompatible auth server: does not support dynamic client registration"**

The OAuth JSON in your config contains spaces or incorrect credentials.

**Fix:** Remove all spaces from the OAuth JSON in your config:

Wrong:

```json
"{\"client_id\": \"abc\", \"client_secret\": \"xyz\"}"
```

Correct:

```json
"{\"client_id\":\"abc\",\"client_secret\":\"xyz\"}"
```

---

**Server-related connection issues**

You may have copied a hashed client secret from an existing OAuth application. Integration Gateway hashes passwords after creation.

**Fix:** Create a new OAuth application and copy the client secret immediately before you save.

---

### Manual OAuth Setup

If the self-service setup in Step 1 fails, create an OAuth 2.0 application manually:

1. Navigate to your Integration Gateway OAuth application settings.
2. Create a new OAuth 2.0 application with these settings:

| Setting | Value |
|---------|-------|
| **User** | Leave empty |
| **Redirect URI** | `http://localhost:6545/oauth/callback` |
| **Client type** | Confidential |
| **Skip authorization** | On |

3. Copy your client ID and client secret before you save. Integration Gateway hashes the secret after creation, so you cannot retrieve it later.
4. Use the credentials to build your configuration manually. For Claude Desktop, add this block to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "IG-MCP": {
      "command": "npx",
      "args": [
        "mcp-remote@0.1.29",
        "https://your-ig-instance.com/integrations/mcp/",
        "6545",
        "--transport",
        "http-only",
        "--static-oauth-client-info",
        "{\"client_id\":\"your_client_id\",\"client_secret\":\"your_client_secret\"}"
      ]
    }
  }
}
```

5. Replace `your-ig-instance.com`, `your_client_id`, and `your_client_secret` with your actual values.

> **No spaces in OAuth JSON:** Remove all spaces from the OAuth JSON string. See the ["Incompatible auth server"](#common-errors) error above for details.

---

### Test from the Command Line

Before you configure your MCP client, verify that the command works in your terminal:

```shell
/opt/homebrew/bin/npx mcp-remote@0.1.29 \
  "https://your-ig-instance.com/integrations/mcp/" \
  "6545" \
  --transport http-only \
  --static-oauth-client-info '{"client_id":"YOUR_ID","client_secret":"YOUR_SECRET"}' \
  --debug
```

A successful connection shows:

```
Token result: { found: true, hasAccessToken: true, ... }
Connected to remote server using StreamableHTTPClientTransport
Proxy established successfully
Press Ctrl+C to exit
```

Press Ctrl+C to stop the test.

---

## Limitations

### Run History Size

- The system caps run history responses at 900 KB to prevent overflow
- Use `get_run_history_item` with search to retrieve data from large run history items

### Client Behavior

- After you register a new user-defined tool through `execute_integration`, you must restart the client to see the tool in the tools list
- The `mcp-remote` proxy must remain active for the connection to stay open

### Django Admin

- The system blocks certain admin models (for example, GlobalConfig) from access
- The system caps results at 200 instances per query
- The system excludes file and image fields from serialization


