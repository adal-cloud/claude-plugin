---
name: create-inbound-server
description: Create a permanent Adal Cloud Inbound webhook URL through the authenticated MCP server and verify regional synchronization.
---

# Create an Inbound webhook receiver

Use this skill when the user wants a permanent webhook URL in their Adal Cloud account.
This MCP does not manage temporary Adal Cloud Go sessions, Outbound jobs, Channels,
Routers, or HTTP Tunnels.

## Prerequisites

Connect a Streamable HTTP MCP client to `https://mcp.adal.cloud/mcp` using
[OAuth or a dedicated MCP token](https://adal.cloud/auth.md). An API token for
`api.adal.cloud` is a different credential. Discover current tool input schemas
through `tools/list`; do not invent settings or use internal service APIs.

## Procedure

1. Identify whether the user wants a new Server or an existing one. Use `list_servers`
   to resolve an existing Server; names are not unique, so retain its UUID.
2. Call `list_regions`. Use the returned `id` UUID as `region_id`, not the region
   code or name. Use the user's chosen region. If they delegate the choice,
   select an available region; otherwise ask when the choice is missing.
3. Call `create_server` with `region_id` and only settings warranted by the task.
   For example, for an explicitly requested POST-only test receiver:

   ```json
   {"region_id":"<UUID returned by list_regions>","name":"Webhook test","methods":["POST"]}
   ```

4. Read the returned UUID and actual HTTPS URL. Call `get_server` and check `synced`
   before declaring the receiver ready or sending a test. If synchronization is
   pending, read again with a bounded wait; report pending state if it persists.
5. If a live test is requested, call `start_webhook_watch` before sending the
   synthetic webhook. Keep `next_cursor`, send only to the returned URL, then
   use `wait_for_webhooks` with the same `server_id` and cursor. Continue with
   each returned `next_cursor`; a wait lasts up to 20 seconds. If the event is
   missed, consult `list_requests` rather than creating another Server.

## Outcome and limits

Return the Server UUID, region, HTTPS URL and synchronization state. A receiver
does not itself configure forwarding to an application. Use the Dashboard for
Channels, Routers and delivery configuration. Traffic balance and product limits
still apply. Do not delete or change an existing Server just to finish a test;
deletion requires the user's request and can be blocked by a Router reference.

See [MCP quickstart](https://adal.cloud/en/docs/mcp/quickstart/index.md),
[Inbound through MCP](https://adal.cloud/en/docs/mcp/inbound/index.md) and
[Server settings](https://adal.cloud/en/docs/inbound/servers/index.md).
