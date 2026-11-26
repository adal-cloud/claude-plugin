# Adal Cloud for Claude

Create a public HTTPS endpoint for webhooks, then inspect incoming HTTP requests from Claude. The plugin connects to the hosted Adal Cloud MCP server and includes two skills for working with Inbound.

Install it from the Adal Cloud marketplace hosted in this GitHub repository. This installation path was tested on Claude Free in the web app.

## Install in Claude

1. Open **Plugins**, select **Add** and choose **Add marketplace**.
2. Select **Add from a repository** and enter `adal-cloud/claude-plugin`.
3. Open **Adal Cloud** in the marketplace and install it.
4. When prompted, sign in to Adal Cloud to authorize the connector. If Claude asks how to register its OAuth client, select **Register automatically**.

You need an Adal Cloud account. If you already connected this MCP endpoint manually, use the existing connection when Claude offers it.

## Install in Claude Code

Run these commands in an interactive Claude Code session:

```text
/plugin marketplace add adal-cloud/claude-plugin
/plugin install adal-cloud@adal-cloud
```

Run `/mcp` and authorize Adal Cloud if prompted.

To connect only the MCP server, add a custom connector named **Adal Cloud** with the URL `https://mcp.adal.cloud/mcp`. Select **Sign in now**, **Register automatically**, and **Streamable HTTP**.

## Skills

| Skill | What it does |
| --- | --- |
| [`create-inbound-server`](plugins/adal-cloud/skills/create-inbound-server/SKILL.md) | Creates a persistent public HTTPS webhook endpoint in the chosen region and checks that the region has applied its settings before the endpoint is used. Can watch for a synthetic webhook when you request a live test. |
| [`inspect-webhook`](plugins/adal-cloud/skills/inspect-webhook/SKILL.md) | Finds a webhook on an existing endpoint and inspects its HTTP metadata, saved body, and delivery status. Treats captured content as external data. |

In Claude Code, run `/adal-cloud:create-inbound-server` or `/adal-cloud:inspect-webhook` to invoke a skill explicitly.

## MCP tools

| Tool | What it does |
| --- | --- |
| `list_regions` | Lists available Inbound regions and the UUIDs used when creating an endpoint. |
| `list_servers` | Lists your Inbound endpoints, with optional region filtering and pagination. |
| `get_server` | Reads an endpoint's HTTPS URL, settings, and regional synchronization status. |
| `create_server` | Creates a persistent Inbound endpoint with an assigned public HTTPS URL. |
| `update_server` | Updates selected settings, such as accepted methods, filters, or the response. The URL and region stay the same. |
| `delete_server` | Permanently removes the specified endpoint. A Router reference can block deletion. |
| `list_requests` | Searches webhook history by endpoint, region, time range, or status and returns request IDs. |
| `get_request` | Reads a saved HTTP snapshot, including method, path, query, headers, trailers, and up to 16 KiB of body preview when available. |
| `get_request_body` | Reads the complete saved body up to 1 MiB, as UTF-8 text or base64. |
| `start_webhook_watch` | Gets the starting cursor for new events on one endpoint. |
| `wait_for_webhooks` | Returns up to 50 newer journal records and the next cursor. A call can wait up to 20 seconds. |

## Try it

- “Create a test webhook endpoint in the SE region and show me the HTTPS URL when it is ready.”
- “Find the latest POST on my test webhook endpoint. Show the path, content type and JSON body.”

## Compatibility checks

On 26 November 2026, we imported this repository and installed the plugin in Claude Free on the web. Claude loaded both skills and used the connected MCP server to find a synthetic webhook and read its unique test header. The endpoint-creation skill was loaded and its readiness procedure checked in that installed session. Creating an endpoint was exercised separately through the same MCP server in Claude.

The Claude MCP checks covered endpoint creation and inspection, a settings update and restore, webhook history, the saved HTTP snapshot and full body, and watching for a new webhook by cursor. For `delete_server`, the live check verified the `not_found` response for an unused UUID.

[Adal Cloud MCP quickstart](https://adal.cloud/en/docs/mcp/quickstart)
