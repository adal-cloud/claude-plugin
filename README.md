# Adal Cloud for Claude

Create a public HTTPS endpoint for webhooks, then inspect incoming HTTP requests from Claude. The plugin connects to the hosted Adal Cloud MCP server and includes two skills for working with Inbound.

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

## Try it

- “Create a test webhook endpoint in the SE region and show me the HTTPS URL when it is ready.”
- “Find the latest POST on my test webhook endpoint. Show the path, content type and JSON body.”

You can also call the skills explicitly in Claude Code with `/adal-cloud:create-inbound-server` and `/adal-cloud:inspect-webhook`.

[Adal Cloud MCP quickstart](https://adal.cloud/en/docs/mcp/quickstart)
