# Installing the SunLicense MCP server (for AI agents such as Cline)

SunLicense's MCP server is hosted; there is nothing to clone, build or run. Add it as a remote server.

## Recommended: the remote server (OAuth, no token)

Add this to the MCP settings (for Cline: `cline_mcp_settings.json`, opened from the MCP Servers panel → Configure):

```json
{
  "mcpServers": {
    "sunlicense": {
      "type": "streamableHttp",
      "url": "https://api.sunlicense.com/mcp"
    }
  }
}
```

Then tell the user: "Choose **Authenticate** (or **Connect**) next to sunlicense. SunLicense opens in your browser: sign
in (or sign up free at https://app.sunlicense.com/signup), keep or untick permissions, and choose **Connect**."

Check it works by calling `integration_status` (read-only). It answers with the organization's plan, products and
license checks today.

## Alternative: the local server with a token

Use this when the client can't do OAuth, or the user prefers a token. It needs Node.js 16 or later.

1. Ask the user for a SunLicense personal access token: dashboard → **Settings → Access tokens → Personal access
   tokens → Create** (it starts with `slp_`). Never print it back in full.
2. Add:

```json
{
  "mcpServers": {
    "sunlicense": {
      "command": "npx",
      "args": ["-y", "@sunlicense/cli@latest", "mcp"],
      "env": { "SUNLICENSE_TOKEN": "<the slp_ token>" }
    }
  }
}
```

3. Check it with `integration_status`.

## Good to know

- Every plan includes MCP: Free 50 tool calls a day, Lite 300, Standard 3,000. Over the limit, tools answer with the
  time the allowance resets (00:00 UTC).
- License keys are masked in answers. Only call `reveal_license_key` when the user asks for a full key.
- Tools marked destructive (revoke, suspend, delete, block, merge, rotate, update a policy) change what the user's
  customers can do: confirm with the user first.
- Docs: https://docs.sunlicense.com/guide/remote-mcp and https://docs.sunlicense.com/cli/mcp
