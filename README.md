## Before You Connect This to a Real System

> **This demo illustrates a potential workflow only. Before connecting any AI tool, LLM, or MCP server to NSPB or other enterprise systems, organizations should obtain approval from their IT, security, and compliance teams and ensure adherence to internal data governance and security policies.**

Mock mode exists so you can try everything with sample data and no credentials. Use it.

---

## What This Does

This MCP server gives AI agents the ability to interact with Oracle EPM Cloud. Instead of manually running Postman calls or EPM Automate commands, you can ask Claude to:

- **"What applications are in my EPM environment?"**

The server translates these natural language requests into Oracle EPM REST API calls.

---


## Architecture

```
You (natural language) → Claude Desktop → MCP Protocol → This Server → Oracle EPM REST APIs → Your EPM Cloud
```

---

## Install

### A: One-Click Install (Recommended)

Available in v2.0.0 and later. Download the Desktop Extension, install it, done. No path editing, no JSON config file.

The current released bundle still shows the old product name. This source change does not publish a new bundle.

1. Download `oracle-epm-cloud-2.0.0.mcpb` from the [latest release](https://github.com/fmepm/oracle-epm-mcp-server/releases/latest).
2. Open Claude Desktop. Go to Settings → Extensions → Advanced → Install Extension.
3. Select the `.mcpb` file.
4. That's it. Mock mode works out of the box. No Oracle credentials needed to test.

For live mode (real EPM environment), configure your environment variables through Claude Desktop's extension settings.

**Step 1: Clone and install**

```bash
git clone https://github.com/fmepm/oracle-epm-mcp-server.git
cd oracle-epm-mcp-server/server
npm ci
```

**Step 2: Configure Claude Desktop**

Open Claude Desktop → Settings → Developer → Edit Config.

Add this to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "oracle-epm-cloud": {
      "command": "node",
      "args": ["/FULL/PATH/TO/oracle-epm-mcp-server/server/index.js"]
    }
  }
}
```

Replace `/FULL/PATH/TO/` with the actual path on your machine.

Windows example:

```json
{
  "mcpServers": {
    "oracle-epm-cloud": {
      "command": "node",
      "args": ["C:\\Users\\Fred\\oracle-epm-mcp-server\\server\\index.js"]
    }
  }
}
```

**Step 3: Restart Claude Desktop**

Quit Claude Desktop completely and reopen it. You should see the MCP tools icon (hammer) in the chat input area. Click it to verify "oracle-epm-cloud" is listed.

**Step 4: Try it**

Type in Claude Desktop:

> "What EPM applications are available in my environment?"

Claude will use the `list_applications` tool and return the mock data.

---

## Switching to Live Mode (Real EPM Environment)

Before you do this in a company environment, read the note at the top of this page: get approval from your IT, security, and compliance teams first.

When you have access to an Oracle EPM Cloud environment, set these environment variables in your Claude Desktop config:

```json
{
  "mcpServers": {
    "oracle-epm-cloud": {
      "command": "node",
      "args": ["/FULL/PATH/TO/oracle-epm-mcp-server/server/index.js"],
      "env": {
        "EPM_MODE": "live",
        "EPM_BASE_URL": "https://epm-YOURDOMAIN.epm.REGION.oraclecloud.com",
        "EPM_USERNAME": "your_username",
        "EPM_PASSWORD": "your_password",
        "EPM_APP_NAME": "Vision"
      }
    }
  }
}
```
