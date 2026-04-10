# Permissions and Security

### Authentication

Integration Gateway uses OAuth 2.0 for authentication. When you first connect, your browser opens an authorization flow. After you authorize, the MCP proxy caches the token for later requests.

To set up credentials, complete the steps in [Configuration](../mcp-server.md#configuration) to generate your OAuth credentials and configure your MCP client.

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
