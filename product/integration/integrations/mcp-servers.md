---
title: "MCP Servers"
description: "Connect external tools and APIs to Forest Admin using the Model Context Protocol"
---

**MCP (Model Context Protocol)** is a standardized way to connect external tools, APIs, and data sources to Forest Admin. By connecting MCP servers, you can extend Forest Admin's capabilities with custom integrations. MCP servers are used in workflows to trigger external actions, fetch data from external systems, and build complex multi-step processes that span multiple systems.

## What is MCP?

The Model Context Protocol is an open standard for connecting AI assistants and applications to external tools and data sources. In Forest Admin, MCP servers enable you to:

- **Extend workflows** - Use external tools within Forest Admin workflows (currently in beta)
- **Access external data** - Query APIs and services directly from Forest Admin
- **Integrate third-party services** - Connect to tools like Slack, GitHub, monitoring systems, and more
- **Build custom integrations** - Create your own MCP servers for proprietary systems

## Connecting an MCP server

To connect an MCP server to your Forest Admin project:

1. Navigate to **Project Settings** → **Integrations**
2. Click **Add MCP Server**
3. Enter your MCP server URL and configuration:
   ```json
   {
     "url": "https://your-mcp-server.com",
     "name": "My Custom MCP Server",
     "apiKey": "your-api-key"
   }
   ```
4. Check if all the tools are exposed
5. Save your configuration

Learn more about MCP servers in the [Claude MCP documentation](https://docs.claude.com/en/docs/agents-and-tools/remote-mcp-servers).
