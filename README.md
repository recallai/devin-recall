# Recall MCP for Devin

A native Devin plugin using Recall's hosted Streamable HTTP MCP server.

- `/recall:usage` — billed usage, bot hours, costs, commitments, and credits.
- `/recall:join-failure` — bot lifecycle, logs, and documented join-failure investigation.

## Connect in Devin Cloud (OAuth)

1. In [Devin Customize → Plugins](https://app.devin.ai/customize), choose **Personal**, then **Add plugin → From repository**. Enter `recallai/devin-recall` (or `https://github.com/recallai/devin-recall`). Leave the subdirectory blank.
2. Wait for indexing, then open **Customize → MCPs → recall → Connect**. Sign in to Recall and complete OAuth.
3. Start a **new session** and ask Devin to call `get_info` and `list_workspaces`. Confirm the intended account and workspace before querying data.

For team installation, choose **Organization** instead. 

OAuth can access the billing accounts and workspaces available to the signed-in Recall user; selecting a default workspace does not restrict that access. For a shared or unattended agent that needs workspace-restricted access, use the scoped MCP API-key alternative below. Plugin installation and skill instructions do not enforce permissions; the credential does.

### Read permissions and billing visibility

Recall OAuth grants read access by default. Keep write and developer permissions disabled for these read-only workflows. For a least-privilege MCP API key, grant:

- `mcp.account.read`
- `mcp.billing.read`
- `mcp.bots.read`
- `mcp.logs.read`
- `mcp.docs.read`

Optionally add `mcp.calendars.read` for calendar-scheduling investigations and `mcp.status.read` for service/provider incidents. Do not grant `mcp.write`, any fine-grained write scope, `mcp.dev`, or developer credential scopes. MCP API-key permissions are immutable; revoke and replace the key to change them.

**Billing data is billing-account-wide.** A workspace-scoped connection must not be described as workspace-isolated billing access. Confirm the intended audience may see account-level billing data before granting the billing-related scopes. The usage skill labels this scope and refuses to substitute a partial bot list for an invoice.

## Alternative: bearer authentication

For shared or unattended agents, create a dedicated key in **Recall Dashboard → Developers → MCP API Keys → New MCP API Key**. Select only the intended workspace and the read scopes above. Use an MCP API key, not a REST API key.

### Devin Cloud

The installed plugin's connection settings come from its source. To use a key without changing the public plugin:

1. Disable its OAuth MCP connection in **Customize → MCPs** so sessions do not retain both credentials.
2. Choose **Add MCP → Add custom MCP**, name it `Recall API key`, and select **HTTP**.
3. Use the MCP URL supplied by the Recall dashboard for that key. Select **Auth Header**, with header key `Authorization` and value `Bearer <your MCP API key>`.
4. Save, run **Test listing tools**, then start a fresh session. Confirm `get_info` and `list_workspaces` expose only the intended workspace. The plugin's skills can use this connection.

