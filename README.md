# SunLicense MCP server

Let your AI assistant run [SunLicense](https://sunlicense.com), the license key service for software people sell
(Minecraft plugins and mods, FiveM resources, Discord bots, desktop apps, games and tools): issue, suspend and revoke
licenses, free a customer's machine, block a leaked key, look up customers, build automations, read license-check
stats, and add a license check to your code.

It's SunLicense's official MCP server, hosted at:

```text
https://api.sunlicense.com/mcp
```

Streamable HTTP with OAuth: the first time a tool runs, your assistant opens SunLicense in the browser, you sign in,
untick any permission you don't want to give, and choose **Connect**. No API key to copy. Prefer a local server with a
token (scripts, CI, no browser)? Use [`sunlicense mcp`](#local-server-sunlicense-mcp) below.

MCP is included in every SunLicense plan: **Free** (50 tool calls a day), **Lite** (300) and **Standard** (3,000).
Sign up free at [app.sunlicense.com](https://app.sunlicense.com/signup).

<!-- Official MCP Registry name, also read by directories that follow it -->
`mcp-name: com.sunlicense/mcp`

## Connect

**claude.ai, Claude Desktop and mobile:** Settings → Connectors → **Add custom connector** → name `SunLicense`, URL
`https://api.sunlicense.com/mcp` → **Connect**.

**ChatGPT** (a plan that allows custom MCP servers): [chatgpt.com/plugins](https://chatgpt.com/plugins) → **Add** →
**Add custom MCP server** → URL as above, authentication **OAuth**, default advanced settings.

**Claude Code:**

```bash
claude mcp add --transport http sunlicense https://api.sunlicense.com/mcp
```

then `/mcp` → **sunlicense** → **Authenticate**. Or install the
[SunLicense plugin](https://github.com/sunlicense-com/claude-plugin), which adds a skill for writing license checks too.

**Cursor:** add to `~/.cursor/mcp.json` (or the project's `.cursor/mcp.json`), then **Connect** in Settings → MCP:

```json
{
  "mcpServers": {
    "sunlicense": { "url": "https://api.sunlicense.com/mcp" }
  }
}
```

**VS Code** (GitHub Copilot agent mode):

```bash
code --add-mcp '{"name":"sunlicense","type":"http","url":"https://api.sunlicense.com/mcp"}'
```

**Windsurf:** add to `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "sunlicense": { "serverUrl": "https://api.sunlicense.com/mcp" }
  }
}
```

**Codex CLI:**

```bash
codex mcp add sunlicense --url https://api.sunlicense.com/mcp
codex mcp login sunlicense
```

**Cline, Gemini CLI and other clients:** add a remote (Streamable HTTP) server with the URL above; any client that
supports MCP OAuth works. It identifies itself with a Client ID Metadata Document or Dynamic Client Registration.

### Local server: `sunlicense mcp`

The same tools run locally from the [`sunlicense` CLI](https://docs.sunlicense.com/cli/) (macOS, Linux, Windows; npm
installs a single binary). Create a personal access token in the dashboard (**Settings → Access tokens**) and add:

```json
{
  "mcpServers": {
    "sunlicense": {
      "command": "npx",
      "args": ["-y", "@sunlicense/cli@latest", "mcp"],
      "env": { "SUNLICENSE_TOKEN": "slp_..." }
    }
  }
}
```

With the CLI installed (`npm install -g @sunlicense/cli`), `sunlicense mcp install` writes this for the assistants it
finds. `SUNLICENSE_API_KEY` (an `slk_` API key) also turns on `test_validate`, a real license check. Both servers share
your organization's daily allowance.

## What it can do

Try: *"Issue a Pro license for jane@example.com"*, *"Why is AX8P-… failing?"*, *"Someone posted my key on a forum,
stop it"*, *"How many license checks failed this week, and why?"*, *"Add SunLicense to this Paper plugin"*.

Prompts: `add_licensing` (set up licensing in this project), `investigate_license`, `block_leak`, `build_automation`.
Resources: SDK snippets (`sunlicense://sdk/{language}`) and the docs.

<!-- tools:start -->
**Products and policies**

| Tool | What it does | |
|---|---|---|
| `list_products` | List products | reads |
| `create_product` | Create a product | changes |
| `delete_product` | Delete a product | changes, asks first |
| `list_policies` | List policies | reads |
| `create_policy` | Create a policy (plan/tier) | changes |
| `update_policy` | Change a policy (plan/tier) | changes, asks first |
| `delete_policy` | Delete a policy | changes, asks first |

**Licenses**

| Tool | What it does | |
|---|---|---|
| `list_licenses` | List licenses | reads |
| `get_license` | Show a license | reads |
| `issue_license` | Issue a license | changes |
| `suspend_license` | Suspend a license | changes, asks first |
| `reinstate_license` | Reinstate a license | changes |
| `revoke_license` | Revoke a license | changes, asks first |
| `delete_license` | Delete a license | changes, asks first |
| `reveal_license_key` | Reveal a license key | changes |
| `rotate_license_key` | Give a license a new key | changes, asks first |
| `offline_license_file` | Offline license file | reads |

**Machines and IP addresses**

| Tool | What it does | |
|---|---|---|
| `list_machines` | List a license's machines | reads |
| `deactivate_machine` | Deactivate a machine | changes, asks first |
| `list_license_ips` | List a license's IP addresses | reads |
| `release_license_ip` | Free a license's IP address | changes, asks first |

**Customers**

| Tool | What it does | |
|---|---|---|
| `list_customers` | List customers | reads |
| `create_customer` | Add a customer | changes |
| `update_customer` | Edit a customer | changes |
| `block_customer` | Block a customer | changes, asks first |
| `unblock_customer` | Unblock a customer | changes |
| `merge_customers` | Merge two customers | changes, asks first |
| `delete_customer` | Delete a customer | changes, asks first |

**Blacklist and exclusion rules**

| Tool | What it does | |
|---|---|---|
| `list_access_rules` | List access rules | reads |
| `add_access_rule` | Add a blacklist or exclusion rule | changes |
| `delete_access_rule` | Delete an access rule | changes, asks first |

**API keys and team**

| Tool | What it does | |
|---|---|---|
| `list_api_keys` | List API keys | reads |
| `create_api_key` | Create an API key | changes |
| `revoke_api_key` | Revoke an API key | changes, asks first |
| `sign_out_member` | Sign a member out | changes, asks first |

**Automations**

| Tool | What it does | |
|---|---|---|
| `automation_catalog` | Automation building blocks | reads |
| `list_automations` | List automations | reads |
| `get_automation` | Show an automation | reads |
| `validate_automation` | Check an automation | reads |
| `create_automation` | Create an automation (switched off) | changes |
| `update_automation` | Change an automation | changes, asks first |
| `test_automation` | Test-run an automation | reads |
| `enable_automation` | Switch an automation on | changes, asks first |
| `disable_automation` | Switch an automation off | changes, asks first |
| `list_automation_runs` | List an automation's runs | reads |

**Stats and setup**

| Tool | What it does | |
|---|---|---|
| `licensing_stats` | Licensing stats | reads |
| `validation_stats` | Validation stats | reads |
| `integration_status` | Integration status | reads |
| `mcp_usage` | MCP calls today | reads |
| `test_validate` | Test a license check (CLI only) | changes |

**Trash**

| Tool | What it does | |
|---|---|---|
| `list_trash` | List the Trash | reads |
| `restore_from_trash` | Restore from the Trash | changes |

**Product analytics**

| Tool | What it does | |
|---|---|---|
| `analytics_summary` | Product analytics summary | reads |
| `analytics_breakdown` | Product analytics breakdown | reads |
| `analytics_features` | Product feature usage | reads |

**Product releases**

| Tool | What it does | |
|---|---|---|
| `list_releases` | List releases | reads |
| `create_release` | Create a release | changes, asks first |

**Code obfuscation**

| Tool | What it does | |
|---|---|---|
| `obfuscate_file` | Obfuscate a file | changes |
| `obfuscation_status` | Obfuscation run status | reads |
| `obfuscation_profiles_list` | List obfuscation profiles | reads |

**Code and docs**

| Tool | What it does | |
|---|---|---|
| `sdk_snippet` | SDK snippet | reads |
| `search_docs` | Search the docs | reads |
<!-- tools:end -->

"Asks first" tools are marked destructive (`destructiveHint`), so assistants ask before running them. Every tool has
a title and explicit read-only, destructive and open-world hints.

## Safety and privacy

- The assistant acts as you: only what your role and the permissions you approved allow. Changes say "Issued by AI
  assistant (…)" on the license.
- License keys stay masked (`AX8P-XXXX-XXXX-XXXX`); a full key is shown only when you ask (`reveal_license_key`).
- SunLicense receives the tool calls only (the tool's name and arguments), never your prompts, chats or code.
- Disconnect any time: dashboard → **Settings → Access tokens → Connected AI apps**. Unused connections end after
  30 days.

Details: [Connect claude.ai, ChatGPT and other apps](https://docs.sunlicense.com/guide/remote-mcp),
[`sunlicense mcp`](https://docs.sunlicense.com/cli/mcp), [privacy policy](https://docs.sunlicense.com/legal/privacy).

## For developers

| | |
|---|---|
| Endpoint | `https://api.sunlicense.com/mcp` (Streamable HTTP, stateless, JSON responses) |
| Auth | OAuth 2.1, authorization code with PKCE (S256); [protected resource metadata](https://api.sunlicense.com/.well-known/oauth-protected-resource/mcp), [authorization server metadata](https://api.sunlicense.com/.well-known/oauth-authorization-server) |
| Clients | Client ID Metadata Documents or Dynamic Client Registration; `https` or loopback redirect URIs |
| Unauthenticated request | `401` with `WWW-Authenticate: Bearer resource_metadata=…` |
| Registry | `com.sunlicense/mcp` in the [official MCP Registry](https://registry.modelcontextprotocol.io) |

This repository holds the listing files (`server.json`, `lhm.plugin.json`, `.cursor-plugin/`, `glama.json`); the
server itself is part of SunLicense Cloud.

## Support

[docs.sunlicense.com](https://docs.sunlicense.com) · [Support](https://docs.sunlicense.com/support) ·
hello@sundevs.net · [status.sunlicense.com](https://status.sunlicense.com)

## License

The files in this repository: [MIT No Attribution](LICENSE). SunLicense itself is a hosted service under its
[terms](https://docs.sunlicense.com/legal/terms).
