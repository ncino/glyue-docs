# MCP Server

The Integration Gateway Model Context Protocol (MCP) server enables AI assistants to interact with integrations directly from any MCP-compatible client. It connects to your Integration Gateway instance and provides tools to read, write, execute, and debug integrations.

Key capabilities:

- Read and search integration structures, such as service requests, field mappings, and validation rules
- Create, update, and delete integrations and their components
- Execute integrations and inspect run history
- Manage Django admin models and build frontends
- Access build helper services 

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

These examples use Claude Desktop and Claude Code. Other MCP-compatible clients might require a different configuration format. Adapt the connection URL, port, and OAuth credentials to your client's requirements.

**Claude Desktop:** Paste the generated configuration directly into `claude_desktop_config.json` in Settings > Developer > Edit Config, then restart Claude Desktop completely. You do not need to make additional changes.

**Claude Code:** The generated configuration does not include a Claude Code command. Run this command in your terminal, and replace the placeholder values with your instance URL and OAuth credentials from Step 1:

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
| "List all available adapters" | Returns the catalog of adapters from the Build Helper service |
| "Get field mapping templates for [service]" | Retrieves pre-built templates for a specific service |
| "Create a new frontend called [name]" | Creates a new CSP-compliant frontend with boilerplate files |

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


