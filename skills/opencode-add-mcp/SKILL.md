---
name: opencode-add-mcp
description: "Manage MCP (Model Context Protocol) servers in OpenCode. Use this skill whenever the user wants to add a new MCP server, configure an existing MCP server, list configured MCPs, debug MCP connections, authenticate with OAuth-enabled MCPs, or manage MCP settings. Trigger phrases: 'add mcp', 'install mcp', 'configure mcp', 'mcp server', 'list mcp', 'mcp auth', 'mcp debug', 'remove mcp', 'update mcp', 'mcp config', 'connect to mcp'."
user-invokable: true
disable-model-invocation: false
---

# MCP Manager

A skill for managing Model Context Protocol (MCP) servers in OpenCode. This skill helps users add, configure, debug, and manage MCP servers to extend Claude's capabilities.

## What are MCP Servers?

MCP servers are external tools that Claude can use to extend its capabilities. They communicate via the Model Context Protocol and can provide additional tools for tasks like:
- Searching documentation (Context7, Grep)
- Managing cloud resources (AWS, GCP, Sentry)
- Interacting with external services (GitHub, Jira, vidIQ)
- And much more

## Quick Start

### Add a Remote MCP Server (API Key auth)
```
Add the vidIQ MCP server at https://mcp.vidiq.com/mcp with API key: vidiq_xxx
```

### Add a Remote MCP Server (OAuth)
```
Add the Sentry MCP server at https://mcp.sentry.dev/mcp
```

### Add a Local MCP Server
```
Add the @modelcontextprotocol/server-everything MCP via npx
```

### List and Debug
```
List all configured MCP servers
Debug the vidIQ MCP connection
```

## Configuration File Locations

OpenCode stores MCP configurations in **one of these locations** (checked in order):

| Priority | Location | Notes |
|----------|----------|-------|
| 1 | `~/.config/opencode/opencode.jsonc` | Preferred - supports comments |
| 2 | `~/.config/opencode/opencode.json` | Alternative location |
| 3 | `~/.opencode/config.json` | Legacy location (fallback) |

**Important**: Check which config file actually exists on the user's system before making changes. The `opencode mcp list` command reveals which configs are being loaded.

## Supported MCP Types

### Remote MCP Servers
- **URL**: The MCP server endpoint (e.g., `https://mcp.vidiq.com/mcp`)
- **Authentication**: API Key (via headers) or OAuth (automatic flow)
- **Headers**: Custom headers for API key auth (e.g., `Authorization: Bearer <key>`)

### Local MCP Servers
- **Command**: How to start the server (e.g., `["npx", "-y", "@modelcontextprotocol/server-everything"]`)
- **Environment**: Optional environment variables

## Workflow

When the user wants to add/manage an MCP server:

1. **Identify MCP type** (remote with API key, remote with OAuth, or local)
2. **Discover the active config file** - check in order:
   - `~/.config/opencode/opencode.jsonc`
   - `~/.config/opencode/opencode.json`
   - `~/.opencode/config.json` (fallback)
3. **Read existing config** from the discovered location
4. **Add/update the MCP** server entry under the `mcp` key
5. **Provide next steps** (restart OpenCode, authenticate, test)

## Configuration Schema

### Remote with API Key
```json
{
  "type": "remote",
  "url": "https://mcp.example.com/mcp",
  "enabled": true,
  "headers": {
    "Authorization": "Bearer YOUR_API_KEY"
  }
}
```

### Remote with OAuth
```json
{
  "type": "remote",
  "url": "https://mcp.example.com/mcp",
  "enabled": true,
  "oauth": {}
}
```

### Local via npx
```json
{
  "type": "local",
  "command": ["npx", "-y", "@modelcontextprotocol/server-everything"],
  "enabled": true
}
```

### Local with environment variables
```json
{
  "type": "local",
  "command": ["npx", "-y", "some-mcp-server"],
  "enabled": true,
  "environment": {
    "API_KEY": "your-api-key"
  }
}
```

## Common MCP Servers Reference

| MCP Server | URL | Auth Type | Description |
|------------|-----|-----------|-------------|
| **vidIQ** | `https://mcp.vidiq.com/mcp` | API Key | YouTube keyword research and analytics |
| **Sentry** | `https://mcp.sentry.dev/mcp` | OAuth | Error tracking and performance monitoring |
| **Context7** | `https://mcp.context7.com/mcp` | API Key (optional) | Documentation search |
| **Grep by Vercel** | `https://mcp.grep.app` | None | Code search on GitHub |
| **Puppeteer** | Local via npm | None | Browser automation |
| **Everything** | `@modelcontextprotocol/server-everything` | None | Test/demo server |

## Commands

After configuring an MCP, use these OpenCode CLI commands:

```bash
# List all MCP servers
opencode mcp list

# Authenticate with an OAuth-enabled MCP
opencode mcp auth <server-name>

# Debug MCP connection issues
opencode mcp debug <server-name>

# Remove stored credentials
opencode mcp logout <server-name>
```

## Using MCP Tools in Prompts

Once configured, you can use MCP tools by referencing them in your prompts:

```
use vidiq to research YouTube keywords for "AI automation"
```

Or add to your AGENTS.md for automatic usage:
```markdown
When researching YouTube content, use the `vidiq` MCP tools.
```

## File Locations

### Config Files (check in this order)
OpenCode may use different config locations depending on version/platform:
- **Primary**: `~/.config/opencode/opencode.jsonc` (supports comments)
- **Alternative**: `~/.config/opencode/opencode.json`
- **Legacy**: `~/.opencode/config.json`

**Verification**: Run `opencode mcp list` - the output reveals which config is active.

### Auth Tokens
- **Stored at**: `~/.local/share/opencode/mcp-auth.json`

## Security Notes

1. **API Keys**: Prefer environment variables over hardcoded keys:
   ```json
   "headers": {
     "Authorization": "Bearer {env:VIDIQ_API_KEY}"
   }
   ```

2. **OAuth**: OpenCode handles OAuth securely with automatic token storage

3. **Local servers**: Only run trusted MCP servers from npm/pip

## Troubleshooting

### MCP not showing up
- Restart OpenCode after adding a new MCP
- Check `opencode mcp list` to verify it's enabled

### Authentication errors
- For OAuth: Run `opencode mcp auth <server-name>`
- For API keys: Verify the key is correct and not expired

### Connection timeouts
- Check the MCP server URL is correct
- Verify network connectivity
- Some MCPs may require VPN or specific network access

## Implementation Notes

When adding/updating an MCP:

1. **Discover the active config location**:
   ```bash
   # Check which configs exist
   ls -la ~/.config/opencode/opencode.jsonc
   ls -la ~/.config/opencode/opencode.json
   ls -la ~/.opencode/config.json
   
   # Verify by running
   opencode mcp list
   ```

2. **Read existing config** from the discovered location (create if none exist, prefer `~/.config/opencode/opencode.jsonc`)

3. **Parse as JSON** (handle both `.json` and `.jsonc` - strip comments if needed)

4. **Add/update the MCP** entry under the `mcp` key

5. **Write back** to the **same config file** that was discovered

6. **Provide clear next steps** to the user

Always validate:
- The MCP server name is unique (or update existing)
- The URL/command is properly formatted
- OAuth is only used for remote MCPs
- Local MCPs have a valid command array

### Common Mistake to Avoid
Don't assume `~/.opencode/config.json` is the active config. OpenCode may use `~/.config/opencode/opencode.jsonc` or `~/.config/opencode/opencode.json` instead. Always verify the active location before making changes!
