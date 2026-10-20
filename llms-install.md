# Install the mailfloss MCP server

mailfloss is a hosted remote MCP server. There's nothing to clone, build or run.

1. Ask the user for their mailfloss API key. They create one in the mailfloss
   dashboard (https://app.mailfloss.com) under **Developers → API keys**. A free
   account includes 500 verifications a month.
2. Add this server to the MCP settings file, replacing `YOUR_API_KEY` with the
   key:

```json
{
  "mcpServers": {
    "mailfloss": {
      "type": "streamableHttp",
      "url": "https://api.mailfloss.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

3. Check the connection by calling the free `get_organization` tool. It returns
   the account's plan and remaining credits.

`verify_email` costs 1 credit per call and `start_batch_verification` costs
1 credit per address, so confirm with the user before verifying a large list.

Docs: https://developers.mailfloss.com/mcp
