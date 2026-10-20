<img src="assets/logo.png" alt="mailfloss" width="96" height="96">

# mailfloss MCP server

mailfloss is an email verification API built for developers and AI agents. Its
remote MCP server lets an agent verify an address before it's saved, verify a
whole list as a batch job and read the results, check credits and usage, and
manage the email platforms mailfloss keeps clean.

- **Endpoint:** `https://api.mailfloss.com/mcp` (Streamable HTTP)
- **Auth:** OAuth sign-in, or your mailfloss API key as a Bearer token
- **Docs:** https://developers.mailfloss.com/mcp

The server is hosted by mailfloss; there's nothing to install or run. This
repository holds the Cursor plugin, an agent skill, and the MCP Registry entry.

## Connect

### Sign in with OAuth

Add the server with no credentials. Your client opens a browser, you sign in to
mailfloss, and you approve the connection.

**Claude Code**

```bash
claude mcp add --transport http mailfloss https://api.mailfloss.com/mcp
```

**Cursor**: add this to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "mailfloss": {
      "url": "https://api.mailfloss.com/mcp"
    }
  }
}
```

**Claude and ChatGPT**: add `https://api.mailfloss.com/mcp` as a custom
connector.

### API key

Create a key in the mailfloss dashboard under **Developers → API keys**, then
send it in the `Authorization` header:

```json
{
  "mcpServers": {
    "mailfloss": {
      "url": "https://api.mailfloss.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" }
    }
  }
}
```

Don't commit the key to source control.

## Tools

| Tool | What it does | Cost |
|------|--------------|------|
| `verify_email` | Verify one address | 1 credit |
| `start_batch_verification` | Verify a list as a background job | 1 credit per address |
| `get_batch_status`, `get_batch_results` | Follow a job and read its verdicts | free |
| `cancel_batch` | Stop a job and refund unprocessed credits | changes the account |
| `get_organization`, `get_account`, `get_usage_report`, `list_jobs` | Plan, credits, usage and job history | free |
| `list_integrations`, `get_integration`, `get_connection`, `test_integration` | Connected email platforms | free |
| `connect_integration`, `update_settings`, `disconnect_integration`, `resync_integration` | Manage connected email platforms | changes the account |

Each tool makes the matching call to the
[mailfloss v1 REST API](https://developers.mailfloss.com/reference) with your
credentials, so credits, rate limits and errors are the same. See the
[MCP guide](https://developers.mailfloss.com/mcp) for details.

## Example prompts

- "Verify sarah@example.com before I add her to the newsletter."
- "Verify these 40 addresses as a batch and tell me which ones are undeliverable."
- "How many credits do I have left this month?"
- "Is my Klaviyo integration still connected, and are its credentials still working?"

## Pricing

mailfloss is free for 500 verifications a month. See
[pricing](https://mailfloss.com/pricing/).

## Support

- Docs: https://developers.mailfloss.com
- Help center: https://docs.mailfloss.com
- Email: hello@mailfloss.com
- Privacy policy: https://mailfloss.com/privacy/
